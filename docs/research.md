---
disqus: ''
template: sidebar.html
hide:
    - footer
---

# 学术研究

## 📄 公开发表
:material-octagram:&nbsp;<a href="https://kns.cnki.net/kcms2/article/abstract?v=Y2E-z2Sa5CMHx2yxy_Xr47EsDX6tFBj0RQUpE18qfZAc4jdNtYeGfM5PRA-p0CzGjmh2pk8CM0oYluyEYXHfDTJ2-HMKLu3LRqhnKo0GMQw_3FjVOKamiq70ynv_A8-sO-gDvwqHAMR63OMWUnnnGzpJ3vtMm3SHc584g05euQ0=&uniplatform=NZKPT" target="_blank">
                        **管理层回复模板化的股价效应——基于“上证 e 互动”的实证研究**, 
                        </a>,合作者: 郑迎飞 和 卞世博，**《会计研究》**,2025. (CSSCI)

> 上市公司和投资者的互动可以帮助投资者获取和理解信息，
> 然而我们发现，在“上证e互动”平台上存在公司管理层采用复制粘贴相同内容的方式来敷衍投资者的现象，
> 我们将这种行为称为“管理层回复模板化”。

> 我们通过利文斯顿编辑距离来度量回复模板化的程度，
> 实证结果表明回复模板化会带来显著的股价下跌，甚至导致股价崩盘。
> 进一步检验发现，回复模板化不仅能够负向预测当期盈余，
> 还会向投资者传递有关公司长期发展的负面信号。

> * **2022年研究生科研创新培育项目**
> * **2022年优秀研究生学位论文培育项目**
> * **2023届研究生优秀学位论文**
> * **“注册制改革与多层次资本市场建设研究生学术论坛”(2022) 二等奖**

[:material-file-code: 模板化度量方法与代码](posts/posts/sim_lev.md)

<div style="text-align: center;">
<img src="/images/research_6.png" width="450" >
</div>

---

:material-octagram:&nbsp;<a href="https://doi.org/10.1016/j.iref.2024.103783" target="_blank">
                         **Navigating uncertainty: The impact of economic policy on corporate data asset allocation**
                         </a>, with Chen Rongda and Jin Chenglu, &nbsp;**_International Review of Economics & Finance_**, 2025. (SSCI Q2)

<div class="annotate" markdown>

> 我们创新地使用了一种基于BERT大语言模型的启发式分词技术(1)，
> 对一系列数据资产相关的研究报告和政策文件进行启发式分词，
> 并利用BERT的衍生模型筛选出数据资产相关术语(2)，构建了一个数据资产术语词典(3)。
> 基于该词典，我们统计了上市公司年报中相关术语的词频，
> 并构造标准化指标以反应公司数据资产配置水平。

> 配置数据资产能够增强企业应对经济政策不确定性的能力，但也需要相应的资源投入。
> 我们希望基于对年报文本的挖掘，探究经济政策环境变化下的企业数据资产配置变化。
> 实证结果表明，平均而言，当经济政策不确定性升高时，企业反而倾向于减少数据资产配置，
> 并且这种效应主要集中在低科技企业。
> 机制分析表明，尽管配置数据资产能够在经济政策环境变化时控制经营成本和经营风险，
> 但在技术水平和财务风险的制约下配置数据资产可能会带来负效益。

> * **在“数据资产定价与金融创新”学术研讨会(2024) 进行报告**

</div>

1. :bulb:传统分词技术(例如jieba)主要基于一个预设的词典来对文本进行分词， 
难以处理超出词典范围的新兴词汇。而启发式分词通常依据字符之间的语义关联性进行分词， 
从而能够将新词正确分离出来。
2. :bulb:筛选过程基于DeBERTa这一衍生模型，该模型具备处理零样本分类(Zero-Shot Classification)任务的能力，
零样本分类是指不需要微调(Fine-Tuning)，直接依靠大语言模型预训练所得权重直接进行推断的能力。
3. :bulb:我们还考虑了一种基于Word2Vec的替代技术方案，
即利用启发式分词后的样本文本来训练（或基于现有模型微调）一组词向量，
然后基于词向量相似度（例如余弦相似度）来筛选出数据资产相关术语。

<a href="/files/epu_digital_asset.pdf" target="_blank">
:fontawesome-solid-file-pdf: 会议报告PPT
</a>

[:material-file-code: BERT启发式分词与代码](posts/posts/digital_asset.md)

[:material-file-code: 爬取上市公司年报代码](posts/posts/fin_report.md)

<div style="text-align: center;">
<img src="/images/research_8.png" width="400" >
</div>

---

:material-octagram:&nbsp;<a href="https://doi.org/10.1016/j.najef.2023.102004" target="_blank">
                         **Systematic COVID risk, idiosyncratic COVID risk and stock returns**
                         </a>, with Wan Xiaoyuan, &nbsp;&nbsp;&nbsp; **_The North American Journal of Economics and Finance_**, 2024. (**Corresponding author**, &nbsp;&nbsp;&nbsp;&nbsp;SSCI Q2)

> COVID-19疫情对全球经济和金融市场产生了深远的影响，但少有文献探究个股在疫情冲击下的风险暴露变化。
> 我们提出了一种基于事件的方法估计COVID-19疫情期间的额外风险，并分析了该风险与未来股票回报的关系。

> 我们使用疫情窗口内个股与市场收益率的超额协方差估计由COVID引发的系统性风险，使用疫情窗口内的超常特质波动率估计由COVID引发的特质性风险。

> 检验发现COVID系统性风险有显著为正的风险溢价，表明对疫情冲击暴露较高的股票，投资者要求更高的风险补偿。
> 而COVID特质性风险与未来股票收益率则没有显著关联，表明疫情冲击引起的公司特有风险能够被投资者充分分散。

> 此外，我们还发现疫情期间的个股超常收益率与未来收益率之间呈现负相关关系，这可能与投资者对疫情冲击的反应过度有关。

[:material-file-code: COVID风险估计方法与代码](posts/posts/COVID_excess_risk.md)

<div style="text-align: center;">
<img src="/images/research_1.png" width="600" >
</div>

---

:material-octagram:&nbsp;<a href="https://doi.org/10.1080/00036846.2023.2298658" target="_blank">
                         **Do stock prices follow random walk over day and night? –– evidence from Chinese stock market**
                         </a>, with Wan Xiaoyuan and Shen Sichao, **_Applied Economics_**, 2023. (**Corresponding author**, SSCI Q2)

> Lou, Polk, and Skouras (2019)指出在日内时段和隔夜时段活跃的交易者属于两个不同的投资者群体，由于二者在行为上存在显著差异，我们据此怀疑日内时段和隔夜时段的股票收益率可能会违背随机游走假设。

> 我们使用了Variance Ratio检验和Filter Rule策略分别在统计上和经济上进行了检验，结果表明两个时段的收益率确实违背了随机游走假设，并且从隔夜时段到日内时段的过程表现出了显著的反转效应（反应过度），而从日内时段到隔夜时段的过程表现出了显著的动量效应（反应不足）。

> 进一步地，考虑到中国市场的T+1制度对隔夜和日内交易行为有较大影响，我们对比了没有实行T+1制度的H股和B股，发现T+1制度能够缓解日内和隔夜时段之间的反应过度和反应不足。

[:material-file-code: 检验方法与代码](posts/posts/vr_and_filter_rule.md)

<div style="text-align: center;">
<img src="/images/research_2.png" width="500" >
</div>

---

:material-octagram:&nbsp;<a href="https://doi.org/10.1016/j.econlet.2022.110509" target="_blank">
                         **The effect of relaxing daily price limit: Evidence from the ChiNext market of China**
                         </a>, with Wan Xiaoyuan, **_Economics Letters_**, 2022. (SSCI Q3, ABS ratings 3)

<div class="annotate" markdown>
> 自1996年12月开始，中国股票沪深两市就施行了10%涨跌幅限制。
> 直到2020年8月24日，注册制在深市创业板进行试点，涨跌幅限制首次从10%放宽到20%.

> 这一准自然实验使得我们能够探究施加涨跌幅限制会有怎样的影响，
> 同期没有进行试点的沪深主板股票依旧遵循10%涨跌幅限制，也为我们使用DID提供了便利。

> 我们发现，在事件日及随后一段时间，市场的超常收益率显著为正。
> 在市场质量方面，涨跌幅限制的放宽没有显著改变价格效率(1)，但显著提高了市场的流动性(2)和收益的波动性(3)。
</div>

1. :bulb:使用$|\rho|$ 和 $|VR|$度量，$|\rho|$ 为个股收益率与滞后市场收益率的相关系数；$|VR|$ 为Variance Ratio的绝对值
2. :bulb:使用$TO$和$ILLIQ$度量，$TO$为股票换手率；$ILLIQ$为Amihud (2002)提出的非流动性指标
3. :bulb:使用$BETA$ 和 $IVOL$度量，$BETA$为市场模型的$\beta$系数；$IVOL$为特质波动率，为市场模型残差的标准差

<div style="text-align: center;">
<img src="/images/research_3.png" width="450" >
</div>

---

:material-octagram:&nbsp;<a href="https://kns.cnki.net/kcms2/article/abstract?v=0rU-DchPtsvK5medBcSPdUANz9-4kwHcCfMm7oLGPMbURNzqxFkZt_sQgVIUhRWPyplH9hHmdl2zBYV0jTEridDS_qyYFWdOWiNRf4gYtPmw0yrb4fhp-zplyAOK8pp-XaNu2gnhoIGi2jyy8RQ6sA==&uniplatform=NZKPT&language=CHS" target="_blank">
                         **网络平台互动、策略性回应与股票错误定价**
                         </a>,合作者: 徐寿福 和 郑迎飞,**《经济管理》**,2023. (CSSCI)

<div class="annotate" markdown>

> 在投资者互动平台上(1)，公司在回复中夹杂大量客套话的回应模式既满足了及时回复的监管要求，
> 又避免了向投资者透露有效信息，使得上市公司能够回避特定信息或隐藏负面消息。
> 我们将这种策略性回应手段称为“避实就虚”。

> 对“避实就虚”策略性回应进行测度后(2)，我们发现这种回应策略会加剧公司股票错误定价，
> 并造成股价低估。进一步研究发现，投资者规模越大、上市公司信息透明度越差，
> 策略性回应对错误定价的驱动效应越显著。

</div>

1. :bulb:即投资者与上市公司进行互动的平台，国内主要的投资者互动平台包括深市“互动易”和沪市“上证e互动”
2. :bulb:主要通过正则表达式捕捉上市公司回复中的客套话

<div style="text-align: center;">
<img src="/images/research_4.png" width="500" >
</div>

---

:material-octagram:&nbsp;<a href="/files/善拿牌照的唯品会：二线电商的互金之路.pdf" target="_blank">
                         **善拿牌照的唯品会——二线电商的互金之路**
                         </a>,合作者:郑迎飞, **全国优秀金融硕士教学案例**， 2021. (国家级案例库，案例编号AL2021055)

> 唯品会作为中国知名的二线电商，通过获取多种金融牌照，积极拓展互联网金融业务。
> 然而，相较于一线电商，唯品会在经营持牌金融业务时面临更大的生存压力和挑战。
> 我们通过案例详细展示了唯品会近十年来争取各种金融牌照的历程，剖析了其关键战略决策，并探讨了其电商业务与金融业务之间的协同与矛盾。

<div style="text-align: center;">
<img src="/images/research_5.png" width="450" >
</div>

## 📝 工作论文
:material-octagram:&nbsp; **什么导致了应收账款逆票据化？——监管升级与“影子票据”替代下的企业行为变化**,合作者: 冯芸，目标期刊：**《金融研究》**. (CSSCI)

> 随着我国票据市场基础设施和监管体系的不断完善，理论上应收账款的票据化趋势应该逐渐增强。
> 然而，企业的实际行为却呈现出“逆票据化”特征，票据化率逐年下降。

> 以2021年《规范商业承兑汇票信息披露有关事宜》的实施作为准自然实验，
> 我们发现，票据信息披露政策显著降低了企业的应收账款票据化率。
> 机制分析表明，票据信息披露带来的市场透明度提升，一方面加强了企业对票据信用风险的甄别，另一方面遏制了违规贴现活动，将具有潜在风险的票据排挤出市场，构成制度变化的短期成本。
> 此外，近年来兴起的“影子票据”对传统票据呈现出明显的替代作用，可能形成规避票据监管的渠道，加剧“逆票据化”的趋势。

> * **2025 CFRN青年金融学者年会 优秀论文**

<a href="/files/CFRN2025_presentation.pdf" target="_blank">
:fontawesome-solid-file-pdf: 会议报告PPT
</a>

<div style="text-align: center;">
<img src="/images/research_10.png" width="550" >
</div>

---

:material-octagram:&nbsp;
                         **(When) is Beta Priced in China?**, with Wan Xiaoyuan, **_Journal of Empirical Finance_** <br>under review. (SSCI Q2, ABS ratings 3)

<div class="annotate" markdown>

> 尽管CAPM模型预测beta较高的股票应该有更高的期望收益率，
> 但是现有研究都表明美国市场的SML(1)可能是平坦的，甚至是向下倾斜的。

> 与美国市场不同，已有的一系列文献均指出，中国市场价格同步性强，并且市场组合在风险因子中扮演重要角色，
> 因此有理由猜测中国市场的beta可能会有正向溢价。

> 的确，我们的实证结果表明，在中国市场beta和股票收益率呈现显著正相关，
> 并且在市场风险较高的子样本中(2)，beta和收益的正相关更强。

> 进一步将日度收益率分为隔夜和日内部分，我们发现beta只在日内存在正溢价，
> 在隔夜时段，beta和收益率则没有显著联系，这一发现与美国市场相反。
> 通过对比AH交叉上市(3)的股票样本，我们发现中国市场呈现的这种beta定价模式可能是由“T+1”交易制度导致的。

</div>

1. :bulb:即证券市场线security market line，描述市场beta与股票期望收益率
2. :bulb:以市场模型$R^2$，EPU指数和宏观经济指标发布作为子样本选择依据
3. :bulb:指公司既在A股上市，又在H股上市，其中A股遵循“T+1”规则，而H股遵顼“T+0”规则

<div style="text-align: center;">
<img src="/images/research_7.png" width="550" >
</div>

---

:material-octagram:&nbsp;**The Overall And Extremely Low Return Spillovers Among Cryptocurrencies and Stock Markets: Evidence from the COVID-19**, 
with Zheng Yingfei, target **_Pacific-Basin Finance Journal_**. (SSCI Q2)

<div class="annotate" markdown>

> 加密货币不受任何国家或政府控制，也没有单一机构或组织控制其流通和发行，
> 作为一种金融资产，加密货币也不存在现金流。这些独特属性使得加密货币与传统资产相对“隔离”。
> 一系列研究也表明加密货币与各类资产的价格动态也相对独立。据此我们猜测加密货币可能具有潜在的避险属性。

> 在COIVD-19对市场带来巨大冲击的背景下，
> 我们探究了代表性加密货币(1)与主要股票市场指数(2)之间的溢出效应。
> 实证结果表明，无论是COVID-19疫情顶峰时期，还是在整体溢出较低的时期，加密货币都扮演冲击的净溢出角色(3)。
> 基于交叉分位数分析(4)的结果表明，即使在出现极端收益率的情况下，加密货币价格和股票指数之间也没有显著的关联性。

</div>

1. :bulb:我们选择了当时市值最高的比特币(Bitcoin)，以太坊(Ethereum)和莱特币(Litecoin)
2. :bulb:我们选择了美国标普500指数(S&P 500)，英国富时100指数(FTSE 100)和中国沪深300指数(SZSE 300)
3. :bulb:采用Diebold and Yilmaz (2009)的Spillover Index方法。
<br><br>Diebold, F.X. and Yilmaz, K., 2009. Measuring financial asset return and volatility spillovers, with application to global equity markets. The Economic Journal, 119(534), pp.158-171.
4. :bulb:即Han et al. (2016)提出的cross-quantilogram方法。 
<br><br>Han, H., Linton, O., Oka, T. and Whang, Y.J., 2016. The cross-quantilogram: Measuring quantile dependence and testing directional predictability between time series. Journal of Econometrics, 193(1), pp.251-270.

<a href="/files/The_Overall_And_Extremely_Low_Return_Spillovers.pdf" target="_blank">
:fontawesome-solid-file-pdf: Abstract & Methodologies
</a>

[:material-file-code: 爬取加密货币行情](posts/posts/crypto_trading.md)

<div style="text-align: center;">
<img src="/images/research_9.png" width="550" >
</div>

## 🧑‍🎓 助研工作
:fontawesome-solid-star: 助研 国家自然科学基金委员会青年项目《中国证券市场的 T+1 交易制度研究：基于隔夜与日内收益率的视角》, 
2023-01至2025-12, 主持人: 万孝园

> **助研成果：**

> :material-octagram:&nbsp;<a href="https://doi.org/10.1016/j.jempfin.2024.101476" target="_blank">
                         **Margin-buying, short-selling, and stock valuation: Why is the effect reversed over time in China?**
                         </a>, Wan Xiaoyuan, **_Journal of Empirical Finance_**, 2024. (SSCI Q2, ABS ratings 3)

> <div class="indented">助研工作：**数据处理**与**实证分析**
    (Table 2到Table10, Figure 2, Figure 3)，助研致谢见论文PDF第1页下方。</div>

---

:fontawesome-solid-star: 助研 教育部人文社会科学研究项目《互联网消费金融创新、风险与监管-基于行为产业组织理论的研究》, 
2018-07至2021-02, 主持人: 郑迎飞

> **助研成果：**

> :material-octagram:&nbsp;<a href="https://doi.org/10.1016/j.pacfin.2023.101971" target="_blank">
                         **Spillover effects between internet financial industry and traditional financial industry: Evidence from the Chinese stock market**
                         </a>, Zheng Yingfei et al., **_Pacific-Basin Finance Journal_**, 2023. (SSCI Q2)

> <div class="indented">助研工作：**实证分析和可视化**(Figure 8和Figure9实证+绘图,
    Figure 1到Figure 7绘图, Figure 10到Figure 13绘图)</div>

> :material-octagram:&nbsp;参与撰写多份关于网络小额贷款的政府专报，其中**三份获得省部级领导批示**。