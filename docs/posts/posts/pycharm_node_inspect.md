---
date: 2023-12-12
authors:
  - zjc
categories:
  - JS逆向
tags:
  - JavaScript
  - 网络爬虫
  - 开发环境
---

# PyCharm与浏览器的连同调试配置
node-inspect提供了一种在浏览器中调试Node.js(1)代码的方法,
通过它就可以使用浏览器调试的同时脱离浏览器环境。
PyCharm与浏览器连同调试使得我们可以在PyCharm中编写代码的同时，
在浏览器中实时查看代码的执行效果和调试信息。
{ .annotate }

1. Node.js 是一个基于 Chrome V8 引擎的 JavaScript runtime，用于在服务器端执行 JavaScript 代码。
它提供了许多服务器端的功能，例如文件系统访问、网络请求处理等。

<div style="text-align: center;">
<img src="/images/pycharm_inspect_6.png" width=800" >
</div>

<!-- more -->

## 安装node-inspect
安装PyCharm和Node.js后，通过npm安装node-inspect：

```bash
npm install -g node-inspect # (1) 
```

1. -g表示global，软件包会被安装到全局的node_modules目录中，
而不是当前项目的 node_modules 目录中

## 配置PyCharm
在PyCharm右上角选择Edit Configurations添加runtime配置：

<div style="text-align: center;">
<img src="/images/pycharm_inspect_1.png" width="300" >
</div>

点击左上角+号添加配置，选择Node.js环境。Node parameters处需要填入`--inspect-brk`，
JavaScript file处填入想要调试的代码文件：

<div style="text-align: center;">
<img src="/images/pycharm_inspect_2.png" width="800" >
</div>

## 测试
在PyCharm中使用刚刚添加的runtime配置执行要调试的JS代码文件，控制台会输出如下内容：

<div style="text-align: center;">
<img src="/images/pycharm_inspect_3.png" width=700" >
</div>

在浏览器中前往`chrome://inspect`，等待Remote Target出现，点击inspect即可进入调试界面：

<div style="text-align: center;">
<img src="/images/pycharm_inspect_4.png" width=700" >
</div>

可以看到在浏览器开发者工具中显示出了PyCharm中编写的代码，并且在断点处断住：

<div style="text-align: center;">
<img src="/images/pycharm_inspect_5.png" width=800" >
</div>
