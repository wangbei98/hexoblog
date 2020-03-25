---
title: Chapter14-Clustering
author: bellick
copyright: true
top: 0
date: 2018-11-16 19:07:17
categories:
- MachineLearning
- MLbyAndrewNg
tags:
- ML
- clustering
mathjax: true
---
# K-means algorithm

input:
* K (number of clusters)
* Training set \\((x^{(1)},x^{(1)},...,x^{(1)})\\)

\\(x^{(i)} \in R^n(drop x_0=1 convention)\\)

![](https://ws2.sinaimg.cn/large/006tNbRwly1fxa4smchbej30qo0f241c.jpg)

# Optimization Objective

![](https://ws2.sinaimg.cn/large/006tNbRwly1fxa55hfcw7j30o10dijug.jpg)

![](https://ws3.sinaimg.cn/large/006tNbRwly1fxa57r2vodj30o20di0vo.jpg)

# Random initialization
![](https://ws4.sinaimg.cn/large/006tNbRwly1fxa59syevfj30nz0dfdho.jpg)

# choose the number of clusters

![](https://ws4.sinaimg.cn/large/006tNbRwly1fxa5jbehxsj30o00dg0u6.jpg)