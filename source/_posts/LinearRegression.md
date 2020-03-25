---
title: LinearRegression
author: bellick
copyright: true
top: 0
date: 2018-11-14 13:42:28
categories:
- MachineLearning
- Models
tags:
- LinearRegression
- ML
mathjax: true
---

# 线性回归
## 1. 一元线性回归模型
\\(y = wx + b\\)
最小化均方误差：
\\(min\sum_{i=0}^m(y_i-y_i^-)\\)
均方误差:
\\(MSE=\frac{1}{m}\sum_{i=0}^m(y_i-y_i^-)\\)
均方根误差：
\\(RMSE = \sqrt{MSE}\\)
回归平方和：
\\(SSR = \sum(y_i^-\-Y)^2\\)
（设样本平均值为Y）
残差平方和：
\\(SSE = \sum(y_i\-y_i^-)^2\\)
总平方和：
\\(SSt = \sum_{i=0}^m(y_i\-Y)\\)
![](https://ws4.sinaimg.cn/large/006tNbRwly1fx7k7y7djlj309s03pgm0.jpg)

𝑅平方表示模型的解释能力，但大小受自变量个数𝑝的影响，随着自变量越多， 𝑅平方只会增加不会减少。调整后𝑅𝑅平方对变量数𝑝𝑝进行惩罚，对结果进行改善。
### 用statesmode 进行分析
* Prob (F-statistic): 模型显著性
* R-squared: 模型解释能力
* Adj. R-squared: 更稳健的模型解释能力
* Skew: 残差分布的偏度
* Kurtosis: 残差分布的峰度
* P>|t|: 自变量显著性
* coef: 自变量权重系数

## 2. 多项式回归
过拟合： low bias, high variance, 对于训练集拟合得很好，对于测试集集合效果很差，可扩展性差
欠拟合: high bias, low variance, 对于训练集和测试集拟合效果都很差。
## 3. 多元线性回归
##### 多重共线性问题
多重共线性是指变量之间存在高度相关关系。可以 通过相关系数矩阵和方差膨胀因子(VIF)判断。Python 中可以使用以下代码查看:

相关系数矩阵:df.corr() 

方差膨胀因子:statsmodels.stats.outliers\_influence.variance_inflation_factor() 

一般来说，VIF大于4，即认为存在多重共线性。
## 4. 岭回归和Lasso回归
向量范数
\\(l_p-norm\\):
\\(||x||_p = p\sqrt{\sum_i|x_i|^p}\\)
\\(l_1-norm\\):
\\(||x||_1 = |x_1|+|x_2|+...+|x_n|\\)
\\(l_2-norm\\):
\\(||x||_2 = \sqrt{|x_1|^2+|x_2|^2+...+|x_n|^2}\\)

OLS 优化目标：
\\({min}_w = ||Xw-y||^2_2\\)
岭回归优化目标：
\\({min}_w = ||Xw-y||^2_2+\lambda||w||_2^2\\)
lasso回归优化目标：
\\({min}_w = ||Xw-y||^2_2+\lambda||w||_1\\)

#### 为什么岭回归和lasso回归可以解决过拟合 与多重共线性问题?
岭回归实质上是一种改良的最小二乘估计法，它损失了无偏性，来换取高的数值稳定性和精度。从本质上讲，岭回归 是带二范数惩罚的最小二乘回归。岭回归 专用于共线性数据分析的有偏估计回归方法，相对于最小二乘法，岭回归是更符合实际、更可靠的回归方法，其对病态数据的拟合要强于 最小二乘法（OLS）

 LASSO回归（least Absolute Shrinkage and Selection Operator）也称为 套索回归，和 岭回归不同的是，Lasso回归在惩罚方程中用的是绝对值，而不是平方。这就决定了LASSO回归 不像岭回归 那样把系数缩小，而是筛选掉一些系数，使得惩罚后的值可能会变成0。

## 5. 梯度下降
