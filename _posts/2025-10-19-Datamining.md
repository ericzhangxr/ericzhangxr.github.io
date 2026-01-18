---
layout: post
title: "Association rule learning"
categories: 课程笔记
math: true
---

# 关联规则

## 指标的含义与计算

Support(A) = (包含A的事务数) / (总事务数)

Confidence(A → B) = Support(A ∪ B) / Support(A)

Lift(A → B) = Confidence(A → B) / Support(B) = Support(A ∪ B) / (Support(A) * Support(B))

提升度 = 1： A和B是独立的，它们之间没有关联。 

提升度 > 1： A和B是正相关的，该规则有效且有价值。 

提升度 < 1： A和B是负相关的，购买A反而会降低购买B的可能性。

## 一个关联规则挖掘的过程是什么样的

在开始挖掘之前，往往有一个minsup与minconf

| 日期 | 股票    |
| ---- | ------- |
| day1 | A B C D |
| day2 | D E     |
| day3 | A B     |
| day4 | A B E F |
| day5 | D E     |

设定一个最小支持度minsup = 40%。也就是说，只有出现频率大于40%的项才会被统计到**频繁项集**内

| 项   | 频数 | 是否统计 |
| ---- | ---- | -------- |
| A    | 3    | 是       |
| B    | 3    | 是       |
| C    | 1    | 否       |
| D    | 3    | 是       |
| E    | 3    | 是       |
| F    | 1    | 否       |

因此，在第一步中，我们找到了频繁一项集{A}{B}{D}{E}

然后，我们在频繁一项集的基础上，去寻找频繁二项集

| 项   | 频数 | 是否统计 |
| ---- | ---- | -------- |
| A B  | 3    | 是       |
| A D  | 1    | 否       |
| A E  | 1    | 否       |
| B D  | 1    | 否       |
| B E  | 1    | 否       |
| D E  | 2    | 是       |

也就是说，找到了两个频繁二项集{A B}{D E}

同时无法生成频繁三项集，也就是说，我们找到了在minsup=40%下的所有频繁项集

{A}{B}{D}{E}{A B}{D E}

下面进行有关minconf=60%的计算

从频繁二项集生成规则

规则: A$\rightarrow$B
$$
\begin{align}
confidence(A\rightarrow B)=\frac {Support(A\cup B)}{Support(A)}=\frac{0.6}{0.6}=1
\end{align}
$$
规则: B$\rightarrow$A
$$
confidence(B\rightarrow A)=1
$$
规则: D$\rightarrow$E
$$
confidence(D\rightarrow E)=\frac{2}{3}
$$
规则: E$\rightarrow$D
$$
confidence(E\rightarrow D)=\frac{2}{3}
$$
我们认为以上关联规则都成立

关联规则{A$\rightarrow$B}的实际含义为：如果今天股票A 上涨，那么今天股票B 也上涨

其余同理

## 新的数据集构造方法

由于新涉及的关联规则是连续两天，因此针对原数据集

| 日期   | 前一天（下标记为1 | 后一天（下标记为2 |
| ------ | ----------------- | ----------------- |
| day1-2 | A B C D           | D E               |
| day2-3 | D E               | A B               |
| day3-4 | A B               | A B E F           |
| day4-5 | A B E F           | D E               |

我们的目标是confidence($A_1 B_1\rightarrow E_2$) = $\frac{support(A_1 B_1 E_2)}{support(A_1 B_1)}$大于我的minconf 即可

通过这种标号方式，可以对新的表进行对应的操作

# AB-TEST

very simple, just like the check experiment in medicine



## steps of abtest

1.purpose of the test

2.indicator we focus

- Core metric: the indicator we want to enhance
- Guardrail metric: the indicator we should **not** sacrifice

3.hypothesis

4.test subjects

5.**test**

6.verification

7.evaluation and iteration

## TEST

### DEFINATION

**Significance Level, $\alpha$**

 a result at least as "extreme" would be very infrequent if the null hypothesis were true.

probability of type I error: when the null hypothesis is true, probability of our experiment appear to REJECT THE NULL HYPOTHESIS

is, reguarly set to be 0.05

**P-VALUE**

Academic Definition: The probability of obtaining the current observed statistic (or an even more extreme one) under the assumption that the null hypothesis ($H_0$) is true.

p-value（英语：p - value）为假设检验中，假设零假设为真时，观测到至少与实际观测样本一样极端的样本的概率。

if  P-value is less than Significance level, i.e., $P<\alpha$, thus we think the outcome is statistically significant, the null hypothesis should be rejected

**Standard Error**

标准误全称：[样本均值](https://zhida.zhihu.com/search?content_id=167748946&content_type=Article&match_order=1&q=样本均值&zhida_source=entity)的标准误(Standard Error for the Sample Mean),顾名思义，标准误是用于衡量样本均值和[总体均值](https://zhida.zhihu.com/search?content_id=167748946&content_type=Article&match_order=1&q=总体均值&zhida_source=entity)的差距。

- **标准差 (Standard Deviation, $\sigma$)**：衡量**单个数据点**离散程度。（这一届考生的分数波动大不大？）

- **标准误差 (Standard Error, SE)**：衡量**抽样结果**的可靠程度。（如果我反复抽样 1000 次，算出 1000 个平均分，这 1000 个平均分的波动大不大？）

$$SE = \frac{\sigma}{\sqrt{n}}$$

**Confidence Interval**

$$CI = \bar{x} \pm Z \times \frac{s}{\sqrt{n}}$$

is another perspective of P-VALUE, essentially, they are same method in judgement



### HOW TO DETERMINE THE SAMPLE SIZE OF THE TEST?





## EXAMPLE OF AB-TEST

1. recommendation
2. trade off between revenue and retention
3. UI
4. Growth Team's recall strategy
5. Technically

## AB-TEST DIFFERENT DISTRIBUTIONS

|                     Assumed distribution                     |                         Example case                         |                        Standard test                         |                       Alternative test                       |
| :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: | :----------------------------------------------------------: |
| [Gaussian](https://en.wikipedia.org/wiki/Normal_distribution) | [Average revenue per user](https://en.wikipedia.org/wiki/Average_revenue_per_user) | [Welch's *t*-test](https://en.wikipedia.org/wiki/Welch's_t-test) (Unpaired *t*-test) | [Student's *t*-test](https://en.wikipedia.org/wiki/Student's_t-test) |
| [Binomial](https://en.wikipedia.org/wiki/Binomial_distribution) | [Click-through rate](https://en.wikipedia.org/wiki/Click-through_rate) | [Fisher's exact test](https://en.wikipedia.org/wiki/Fisher's_exact_test) | [Barnard's test](https://en.wikipedia.org/wiki/Barnard's_test) |
| [Poisson](https://en.wikipedia.org/wiki/Poisson_distribution) |                 Transactions per paying user                 | *E*-test[[9\]](https://en.wikipedia.org/wiki/A/B_testing#cite_note-9) |                           *C*-test                           |
| [Multinomial](https://en.wikipedia.org/wiki/Multinomial_distribution) |               Number of each product purchased               | [Chi-squared test](https://en.wikipedia.org/wiki/Chi-squared_test) |       [*G*-test](https://en.wikipedia.org/wiki/G-test)       |
|                           Unknown                            |                                                              | [Mann–Whitney *U* test](https://en.wikipedia.org/wiki/Mann–Whitney_U_test) | [Gibbs sampling](https://en.wikipedia.org/wiki/Gibbs_sampling) |

Student's t-test: i.i.d, the Population is Gaussian Distribution, homoschedasticity.

Welch's t-test does not assume homoschedasticity 

Fisher's exact test: based on Hypergeoetric Dristribution, deal with the contingent tablel

$$P(X=a) = \frac{\binom{a+b}{a} \times \binom{c+d}{c}}{\binom{n}{a+c}}$$

**Fisher 的逻辑 (Conditional)**：它假设表格的**行合计 (Row Totals)** 和 **列合计 (Column Totals)** 都是**固定不变**的（Fixed Margins）。

**Barnard 的逻辑 (Unconditional)**：它认为在现实的 A/B 测试中，**只有一边的合计是固定的**（通常是样本量），而另一边的合计（结果分布）是随机的。

Gaussian v.s. Poisson

- Gaussian: mean-based indicators, continuous variables, such as ARPU, AOV, sojourn time
- Possion: Frequenct in a unit time, such as Transactions per paying user

