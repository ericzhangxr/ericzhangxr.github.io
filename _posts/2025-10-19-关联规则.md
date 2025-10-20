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
