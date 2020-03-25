---
title: LogisticRegression
author: bellick
copyright: true
top: 0
date: 2018-11-14 19:04:11
categories:
- MachineLearning
- Models
tags:
- LinearRegression
- ML
mathjax: true
---
# 逻辑回归（对数几率回归）

## 1. 逻辑回归原理
\\(Cost = -ylog(h(x))-(1-y)log(1-h(x))\\)
#### 离散变量的处理
* One-hot、
* Label-encoding、
* get_dummies
* One-hot: sklearn.preprocessing. OneHotEncoder 
* Label-encoding: sklearn.preprocessing. LabelEncoder 
* Get_dummies: pandas.get_dummies

## 2. 模型评价
#### 混淆矩阵 [这儿](../../12/Chapter12-MachineLearningSystemDesign/)
FP为一类错误，FN为二类错误
## 3. 不平衡集问题
## 4. 解决多分类问题
#### 1. OneVsAll/OneVsRest
#### 2. Softmax
[这儿](../../13/Softmax/)

## 5. 非线性分类问题