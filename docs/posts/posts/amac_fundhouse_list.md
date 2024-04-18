---
date: 2024-04-12
categories:
    - 技术
tags:
    - 网络爬虫
---

# 中基协-基金管理人名录数据
中国证券投资基金业协会，aka 中基协，提供了大量基金行业的数据。其<a href="https://www.amac.org.cn/fwdt/wyc/jgcprycx/jgcx/" target="_blank">服务大厅</a>提供了有关机构、产品和人员数据的查询服务。这篇文章介绍如何爬取**公募基金管理人名录**数据。

<!-- more -->

## 数据接口
我们先尝试获取数据接口， 从服务大厅的机构查询入口可以找到<a href="https://www.amac.org.cn/fwdt/wyc/jgcprycx/jgcx/gmjjglrml/" target="_blank">公募基金管理人名录</a>查询。在我撰写这篇文章时共有158条记录，共16页，数据量不大。

<div style="text-align: center;">
<img src="/images/amac_fundhouse.png" width="650" >
</div>

!!! warning
    
    在该页面打开浏览器开发者工具可能会自动进入了调试模式，阻止我们对页面元素进行审查。在调试工具中可以看到是执行了`assets`目录下`0b0c58b7_chunk.js`这段代码导致的，我们右键点击这个文件，选择**Add script to ignore list**就可以正常审查页面元素了。
    <div style="text-align: center;">
    <img src="/images/amac_fundhouse_debugger.png" width="300" >
    </div>

接着将开发者工具切换到**Network**标签，选择**Fetch/XHR**类型，刷新页面，可以看到两条记录，其中有`pageNo=1&pageSize=10`字样的就是数据接口。点击该记录查看**Headers**，可以看到数据接口为`https://www.amac.org.cn//portal/front/mutualFund/findMutualFundHousePage?` ，query为`pageNo=1&pageSize=10`.
<div style="text-align: center;">
<img src="/images/amac_fundhouse_api.png" width="650" >
</div>

## 请求数据
使用urllib库构造URL，我们刚刚看到总共有158条记录，所以尝试将query中的pageSize改为200，使得一次请求就可以将所有数据提取出来。

```python
import urllib
api = "http://www.amac.org.cn//portal/front/mutualFund/findMutualFundHousePage?"

query = {"pageNo": 1, "pageSize": 200}
query = urllib.parse.urlencode(query)

url = api + query
url

# output
# 'http://www.amac.org.cn//portal/front/mutualFund/findMutualFundHousePage?pageNo=1&pageSize=200'
```

尝试请求该链接，会报SSLError错误：UNSAFE_LEGACY_RENEGOTIATION_DISABLED

```python
import requests

requests.get(url)

# output
# ... SSLError: [SSL: UNSAFE_LEGACY_RENEGOTIATION_DISABLED] ...
```

这是因为网站使用了旧版本的协议，与requests产生了冲突，stack overflow的帖子提供了<a href="https://stackoverflow.com/a/73519818/15903747" target="_blank">解决方法</a>：

```python
import requests
import urllib3
import ssl


class CustomHttpAdapter (requests.adapters.HTTPAdapter):
    # "Transport adapter" that allows us to use custom ssl_context.

    def __init__(self, ssl_context=None, **kwargs):
        self.ssl_context = ssl_context
        super().__init__(**kwargs)

    def init_poolmanager(self, connections, maxsize, block=False):
        self.poolmanager = urllib3.poolmanager.PoolManager(
            num_pools=connections, maxsize=maxsize,
            block=block, ssl_context=self.ssl_context)


def get_legacy_session():
    ctx = ssl.create_default_context(ssl.Purpose.SERVER_AUTH)
    ctx.options |= 0x4  # OP_LEGACY_SERVER_CONNECT
    session = requests.session()
    session.mount('https://', CustomHttpAdapter(ctx))
    return session

```

通过上述`get_legacy_session`函数就可以成功请求到数据，我们将传回的数据转换为json

```python
import json

r = get_legacy_session().get(url)
data = json.loads(r.text)
data

# output
# {'path': '//portal/front/mutualFund/findMutualFundHousePage',
#  'code': 200,
#  'data': {'errcode': 0,
#   'msg': '查询成功',
#   'data': {'dataList': [{'lineId': '1',
#      'houseName': '国泰基金管理有限公司',
#      'registerAddr': '上海',
#      'officeAddr': '上海',
#      'website': 'www.gtfund.com',
#      'phone': '4008888688/\n021-31089000'},
# ...
```

json中data-data-dataList下就是我们需要的数据。最后，我们可以将其转换成DataFrame:

```python
import pandas as pd

df = pd.DataFrame(data["data"]["data"]["dataList"])
df

# output
#     lineId         houseName registerAddr officeAddr           website  \
# 0        1        国泰基金管理有限公司           上海         上海    www.gtfund.com   
# 1        2      南方基金管理股份有限公司           深圳         深圳    www.nffund.com   
# 2        3        华夏基金管理有限公司           北京         北京  www.chinaamc.com   
# 3        4        华安基金管理有限公司           上海         上海  www.huaan.com.cn   
# 4        5        博时基金管理有限公司           深圳         深圳    www.bosera.com   
# ..     ...               ...          ...        ...               ...
```

