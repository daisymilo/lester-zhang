---
date: 2024-06-18
categories:
  - 爬虫
tags:
  - 网络爬虫
  - 数据接口
---

# CoinMarketCap加密货币行情爬取
在**The Overall And Extremely Low Return Spillovers Among Cryptocurrencies and Stock Markets: Evidence from the COVID-19**
这篇论文中，我们探究了COVID-19疫情期间加密货币与股票市场之间的溢出效应。

我们使用的加密货币历史行情数据来源于
<a href="https://coinmarketcap.com/" target="_blank">
CoinMarketCap
</a>
，该站提供了丰富的加密货币的行情数据，现在介绍如何将这些数据爬取下来。

<div style="text-align: center;">
<img src="/images/coinmktcap_3.png" width="1000" >
</div>

<!-- more -->

## 接口分析
打开任意加密货币的历史行情页面，例如
<a href="https://coinmarketcap.com/currencies/bitcoin/historical-data/" target="_blank">
Bitcoin
</a>.

<div style="text-align: center;">
<img src="/images/coinmktcap_2.png" width="1000" >
</div>

可以看到尽管页面提供了数据下载功能，但单次最多下载400条数据。 使用浏览器开发者工具进行抓包可以找到如下数据接口：

```python
https://api.coinmarketcap.com/data-api/v3.1/cryptocurrency/historical?id=1&convertId=2781&timeStart=1514764800&timeEnd=1517356800&interval=1d
```

其中有五个个关键参数：

| 参数         | 含义                               |
|:-----------|:---------------------------------|
| id         | 加密货币id，id=1表示Bitcoin             |
| convertId  | 兑换货币的类型，convertId=2781表示加密货币兑美元  |
| timeStart  | 查询起始时间，以时间戳表示                    |
| timeEnd    | 查询结束时间，以时间戳表示                    |
| interval   | 数据频率，interval=1d表示日度             |

只需要根据数据需求选择对应的参数构造url即可请求到数据。

## Python代码
首先需要按照对应参数拼接url，我们构造如下函数，传入起止日期的时间戳，加密货币id，返回url：

```python
def make_url(stamp_start, stamp_end, crypto_id, convert_id=2781):
    url = f"https://api.coinmarketcap.com/data-api/v3.1/cryptocurrency/historical?id={crypto_id}&convertId={convert_id}&timeStart={stamp_start}&timeEnd={stamp_end}&interval=1d"
    return url
```

我们希望输入形如"2020-01-01"这样格式的起止日期，而不是时间戳，
所以构造函数来将输入的字符串时间处理成时间戳：

```python
def make_time_range(start, end):
    start = pd.to_datetime(start)-pd.offsets.Day(1)
    end = pd.to_datetime(end)
    date_range = pd.date_range(start, end, freq="Y").to_list()
    date_range = [start] + date_range + [end]
    date_range = np.array(sorted(list(set(date_range))))
    starts = date_range[:-1]
    starts = [int(i.timestamp()) for i in starts]
    ends = np.roll(date_range, -1)[:-1]
    ends = [int(i.timestamp()) for i in ends]
    return list(zip(starts, ends))
```

在抓包时可以看到服务器返回的数据格式是这样的：

<div style="text-align: center;">
<img src="/images/coinmktcap_1.png" width="800" >
</div>

编写一个函数将JSON数据解析为DataFrame：
```python
def parse_json(data):
    code = data["data"]["id"]
    name = data["data"]["name"]
    symbol = data["data"]["symbol"]
    quotes = data["data"]["quotes"]
    if len(quotes) == 0:
        return None
    record_ls = []
    for record in quotes:
        record_ls.append(record["quote"])
    df = pd.DataFrame(record_ls)
    df["timestamp"] = pd.to_datetime(df["timestamp"].str[:10])
    df["name"] = name
    df["code"] = code
    df["symbol"] = symbol
    return df
```

最后将上述步骤封装到一个函数内：

```python
def crawl_and_parse(time_range, code):
    df_ls = []
    for start, end in tqdm(time_range):
        url = make_url(start, end, code)
        # print(url)
        r = requests.get(url)
        data = json.loads(r.text)
        df_ls.append(parse_json(data))
        time.sleep(2)
    return pd.concat(df_ls)
```

测试我们的代码，尝试爬取Bitcoin的历史行情并保存：
```python
code = 1
start = "2010-01-01"
end = "2024-03-17"
time_range = make_time_range(start, end)
res = crawl_and_parse(time_range, code)
res.to_excel(f"./数据/Code_{code}.xlsx", index=False)
```