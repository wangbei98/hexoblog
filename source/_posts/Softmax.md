---
title: 1.Softmax
copyright: true
top: 0
date: 2018-11-13 09:22:02
tags:
  - softmax
  - MachineLearning
categories:
  - MachineLearning
  - Concepts
mathjax: true
---

# Softmax 回归
1. 在logistic regression 中，我们的训练集由 m 个已标记的样本构成：\\(((x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}...(x^{(m)},y^{(m)}))\\) 输入特征 \\(x^{(i)}\\)
其中输入特征 \\(x^{(i)}\in Re^{n+i}\\)
\\(x_0 = 1\\) 截距项
对于二分类问题， \\(y{(i)}\in (0,1)\\)
假设函数如下
\\(H\_\theta(x) = \frac{1}{1+exp(-\theta^Tx)}\\)
我们将训练参数 \\(\theta\\)，使其能够最小化代价函数：
$$J(\theta = -\frac{1}{m}[\sum_{i=1}^{m}y{(i)}logh\_\theta(x^{(i)})+(1-y{(i)})log(1-h\_\theta(x{(i)}))]$$

2. 在softmax回归中，我们解决的是多分类问题，类标\\(y\\) 可以取\\(k\\)个不同的值。因此对于训练集\\(((x^{(1)},y^{(1)}),(x^{(2)},y^{(2)}...(x^{(m)},y^{(m)}))\\)， 我们有 \\(y{(i)}\in (1，2,...k)\\)。

对于给定的输入\\(x\\)， 我们想用假设针对每个类别估算出概率值 \\(p(y=j|x)\\)。 也就是说，我们想估计\\(x\\)的每一种分类结果出现的概率。因此我们要输出一个k维向量，并且向量元素的和为 1， 来表示这k个估计的概率值。


![](https://gss3.bdstatic.com/7Po3dSag_xI4khGkpoWK1HF6hhy/baike/s%3D353/sign=4c1d9e31be119313c343f9b556390c10/cf1b9d16fdfaaf51ec80d14c805494eef11f7ae5.jpg)

其中 \\(\theta_1,\theta_2,...\theta_k\in Re^{n+1}\\)是模型的参数。注意 ![](https://gss1.bdstatic.com/9vo3dSag_xI4khGkpoWK1HF6hhy/baike/s%3D80/sign=eed7f44cacec08fa22001ea75bee9ac6/8435e5dde71190ef6f12c22cc21b9d16fcfa6053.jpg) 这一项对概率进行归一化，使得所有概率之和为一。

在实现Softmax回归时，将\\(\theta\\) 用一个\\(k\\)x\\((n+1)\\) 的矩阵来表示会更方便：
![](https://gss2.bdstatic.com/-fo3dSag_xI4khGkpoWK1HF6hhy/baike/s%3D95/sign=7feadf62c111728b342d8027cafc64e3/b999a9014c086e06b37f6fb80e087bf40bd1cb6a.jpg)