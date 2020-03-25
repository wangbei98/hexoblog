---
title: Chapter12-MachineLearningSystemDesign
author: bellick
copyright: true
top: 0
tags: 
categories:
  - MachineLearning
  - MLbyAndrewNg
date: 2018-11-12 14:59:00
mathjax:
---

# 1.Priori3zing what to work on: Spam classifica3on example

# 2.Error analysis 

# 3. Error metrics for skewed classes

#### 对于二分类问题

|||avtual class||
|:--|:--|:--|::--|--|
|||1|0|
|predict|1|True Positive|Flase positive|
|class|0|Flase negative|True negative|

precision(查准率)= TruePositives/#predictPositives=TruePositive/TruePositive+FalsePositive

查准率：预测的正例中有多少真的是正例

recall(查全率)=
TruePositive/#actualPositives=TruePositive/TruePositive+FalseNegative

查全率：真正的正例中有多少被正确地预测出来。

#### 如何平衡precision和recall

F1 Score : precision 和 recall 的调和平均
![](https://ws1.sinaimg.cn/large/006tNbRwly1fx5c3uxlx1j305p023wem.jpg)

# 4. Data for machine learning
1. many parameters (e.g. logis3c regression/linear regression with many features; neural network with many hidden units) --> low bias 
2. Use a very large training set (unlikely to overfit)-->  low variance