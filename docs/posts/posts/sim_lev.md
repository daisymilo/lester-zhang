---
date: 2024-01-15
authors:
  - zjc
categories:
  - 数据处理与分析
tags:
  - Python
  - 工作论文
  - 实证
  - 自然语言处理
---

# 文本相似度——利文斯顿编辑距离
在**管理层回复模板化的股价效应——基于“上证 e 互动”的实证研究**这篇论文中，
我们使用利文斯顿编辑距离来度量投资者互动平台上公司管理层回复的模板化程度，
现在对这一方法进行介绍。

$$Sim\_Lev = 1 - [LEV(TEXT_1, TEXT_2) / max(Len(TEXT_1), Len(TEXT_2))]$$

<!-- more -->

## 基本原理
利文斯顿编辑距离衡量的是两段文本的差异程度，
其度量的是其中一段文本通过插入、删除或替换单个字符从而转换成另一段文本的最小步骤数目。

例如，现在有两段文本：

|    文本A     |     文本B      |
| :----------: | :------------: |
| 花花是一条狗 | 花花是一只猫咪 |

将文本A转换为文本B需要如下3步：

1. 花花是一**条**狗 $\Rightarrow$ 花花是一**只**狗 （将“条”替换为“只”）
2. 花花是一只**狗** $\Rightarrow$ 花花是一只**猫** （将“狗”替换为“猫”）
3. 花花是一只猫 $\Rightarrow$ 花花是一只猫**咪** （插入“咪”）

因此，文本A与B的利文斯顿编辑距离就等于3。 我们可以将计算出的利文斯顿编辑距离除以两段文本中较长者的长度，
将取值范围缩放到[0, 1]，再用1减去这个值，得到的就是对两段文本模板化程度的度量：

$$Sim\_Lev = 1 - [LEV(TEXT_1, TEXT_2) / max(Len(TEXT_1), Len(TEXT_2))]$$

该指标取值越大表明两段文本由复制粘贴得来而不做修改的程度越高。

## Python代码
在Python中可以使用
<a href="https://jamesturk.github.io/jellyfish/" target="_blank">
jellyfish
</a>
库来计算利文斯顿编辑距离，
我们编写一个函数计算上面提到的指标：

```python
import jellyfish

def norm_distance(text1, text2):
    # 计算利文斯顿编辑距离
    distance = jellyfish.levenshtein_distance(text1, text2)
    # 取文本较长者的长度
    length = max(len(text1), len(text2))
    return 1 - distance/length
```

在我们的论文中，考虑一家公司在季度频率下的回复模板化度量，
方法是将一个季度内公司的所有回复进行一一匹配，计算上述指标，然后取平均值，
编写下列函数以进行匹配并计算指标取其平均值：

```python
import pandas as pd

def quartly_distance(series):
    # 将输入的一个季度的全部回复内容进行一一匹配
    iv = list(zip(series.index, series.values))  # 将每一条回复保存为(索引, 内容)的格式
    pairs = (pd.MultiIndex.from_product([iv, iv]) # 将所有回复匹配
             .to_series()
             .reset_index(drop=True))
    pairs = pairs[pairs.apply(lambda x: x[0][0] != x[1][0])] # 去除自身匹配自身的记录
    
    # 计算每个配对的指标值
    pairs_distance = pairs.apply(lambda x: norm_distance(x[0][1], x[1][1]))
    # 取平均值返回
    return pairs_distance.mean()
```

我们的数据包含三个字段，**sec_code**表示股票代码，**a_date**表示回复日期，**a_context**表示回复文本

<div style="text-align: center;">
<img src="/images/sim_lev_1.png" width="800" >
</div>

根据股票代码和季度进行groupby，然后再调用上面编写的函数就可以得到所需的结果，
修改freq参数可以轻松地对其他频率下的模板化程度进行聚合：

```python
result = (data
          .groupby(["sec_code", pd.Grouper(key="a_date", freq="QE")])["a_context"]
          .apply(quartly_distance))
```

!!! warning

    我们在论文中使用的沪市“上证e互动”和深市“互动易”问答数据分别达到了55万条和100万条，
    考虑到还要进行记录间的一一匹配，计算量较大，推荐使用下列多进程对计算过程进行加速。

下面代码为上述方法的多进程执行版本，显著加快了计算速度，并添加了进度条：

```python
import joblib
from joblib import Parallel, delayed
import contextlib
from tqdm.notebook import tqdm

@contextlib.contextmanager
def tqdm_joblib(tqdm_object):
    class TqdmBatchCompletionCallback(joblib.parallel.BatchCompletionCallBack):
        def __call__(self, *args, **kwargs):
            tqdm_object.update(n=self.batch_size)
            return super().__call__(*args, **kwargs)

    old_batch_callback = joblib.parallel.BatchCompletionCallBack
    joblib.parallel.BatchCompletionCallBack = TqdmBatchCompletionCallback
    try:
        yield tqdm_object
    finally:
        joblib.parallel.BatchCompletionCallBack = old_batch_callback
        tqdm_object.close()

groups = data.groupby(["sec_code", pd.Grouper(key="a_date", freq="QE")])["a_context"]

with tqdm_joblib(tqdm(desc="progress", total=len(groups))) as progress_bar:
    results = (Parallel(n_jobs=joblib.cpu_count())
               (delayed(quartly_distance)
                (series=g) for n, g in groups))
```