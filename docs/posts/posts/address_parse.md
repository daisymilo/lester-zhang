---
date: 2024-04-18
categories:
  - 数据处理与分析
tags:
  - 文本分析
  - Python
  - JavaScript
---

# 公司地址智能解析（基于JavaScript接口）
在研究当中可能需要用到公司地址信息，尽管很多数据库都提供这一数据，但其对公司所处行政区划的细分可能不符合我们的要求。例如，我们想获取省、市、区三级行政区划，但CSMAR数据库没有相应的细分数据，只能自行构建。

这篇文章介绍如何使用现成的JavaScript库部署接口，从而在Python中调用接口进行公司地址的智能解析。

<!-- more -->

自行提取行政区划的可行思路是将公司地址进行切分，然后与全国行政区划进行对照，但我们其实不应该在研究中这种细枝末节的地方花费太多时间精力。联想到快递寄件时基本都有智能地址解析功能，说明这一技术已经成熟。
<div style="text-align: center;">
<img src="/images/address_parse_app.jpg" width="450" >
</div>
的确，GitHub上有不少国内地址解析的项目，但大多都基于JavaScript语言，这一语言通常活跃在互联网应用领域，我们接触不多。好在我们可以通过简单几行JavaScript代码部署一个智能地址解析的接口，然后在Python当中通过这个接口来对我们的公司地址数据进行解析。

## 部署接口-JavaScript
我选择了<a href="https://github.com/ldwonday/zh-address-parse/tree/master" target="_blank">zh-address-parse</a>这个智能地址解析项目，要将其部署首先需要安装JavaScript开发环境<a href="https://nodejs.org/en/download" target="_blank">Node.js</a>。

安装完成后在任意位置新建一个文件夹用来放置接口所需要的代码。然后，我们要安装所需的JavaScript库。在**刚刚新建的文件夹**下打开命令行，输入下列命令安装用于部署接口的express库和刚刚提到用于智能地址解析的zh-address-parse库：
```shell
npm install express

npm i zh-address-parse -s
```

接着，我们编写接口的JavaScript代码，JS也属于面向对象编程的语言，与python相似。在文件夹下新建文本文档，命名为`app.js`，写入下列代码：
```javascript
// 导入express框架
const express = require('express');

// 导入刚刚安装的地址解析库
const AddressParse = require('./node_modules/zh-address-parse/dist/zh-address-parse.min.js')
```

!!! tip

    npm所下载的框架/库会放置在node_modules文件夹下，对于zh-address-parse这个库我们只导入了`zh-address-parse.min.js`这个文件，`.min.js`表明该文件包含了这个库的完整功能

我们继续向`app.js`中写入代码：
```javascript
const app = express();  // 实例化express对象，取名为app，它就是我们的服务器
app.use(express.json());  // 服务器接收json格式的输入

// 定义向服务器的/parse路径进行发送的post请求
app.post('/parse', (req, res) => {
    // post请求中需要包含options和address两个属性
    const address = req.body.address; // address即为待解析地址
    const options = req.body.options; // options为解析时的一些选项
    // 将address和options传入地址解析库AddressParse，返回的结果保存在result变量里
    const result = AddressParse(address, options);
    // 定义post的相应为json格式，内容为result
    res.json(result);
});

// 定义向服务器根目录发送的get请求
// 用来测试我们的服务是否正常工作：对根目录发送get请求时，显示一些提示文字
app.get('/', (req, res) => {
  res.send('服务正在运行');
});

// 将服务器映射到本地的3000端口上
const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Parse server running on port ${PORT}`);
});
```

编写完JavaScript代码之后，在文件夹中打开命令行输入`node app.js`即可运行我们的接口，在浏览器中访问`http://localhost:3000/` 便可以测试我们的接口运行情况。

## 调用接口-Python
接口部署完毕后，我们就可以在python中调用接口了。我们需要构造一个字典，包含address和options两个键，address对应的值就是我们待解析的地址，而options对应的值是解析时的选项，应该参考<a href="https://github.com/ldwonday/zh-address-parse/tree/master?tab=readme-ov-file#usage" target="_blank">zh-address-parse的说明</a>：

```python
address = "广东省深圳市福田区益田路5023号平安金融中心B座"

# 根据zh-address-parse的说明：
options = {"type": 1, # 哪种方式解析，0：正则，1：树查找
           "textFilter": [], # 预清洗的字段
           "nameMaxLength": 4, # 查找最大的中文名字长度
           "extraGovData": {
               "city": [{"name": "name", "code": "code", "provinceCode": "provinceCode"}],
               "province": [{"name": "name", "code": "code"}],
               "area": [{"name": "name", "code": "code", "provinceCode": "provinceCode", "cityCode": "cityCode"}]
           }
          }

json = {"address": address,
        "options": options}
```

最后，我们使用requests库对接口的url`http://localhost:3000/parse` 发送post请求。在返回的结果中，province，city和area就对应了省、市、区三级行政区划：
```python
url = "http://localhost:3000/parse"
response = requests.post(url, json=json)
response.json()

# output
# {'phone': '',
#  'province': '广东省',
#  'city': '深圳市',
#  'area': '福田区',
#  'detail': '益田路5023号平安金融中心B座',
#  'name': '',
#  'postalCode': '',
#  'areaCode': '440304'}
```

!!! warning

    如果Python是运行在Docker容器当中，`http://localhost:3000/parse`对应的是虚拟机自身的本地3000端口，但地址解析接口是部署在宿主机的3000端口上，因此需要将url改为`http://host.docker.internal:3000/parse`以表明是向宿主机进行请求