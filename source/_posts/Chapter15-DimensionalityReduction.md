---
title: Chapter15-DimensionalityReduction
author: bellick
copyright: true
top: 0
date: 2018-11-21 17:15:44
categories:
- MachineLearning
- MLbyAndrewNg
tags:
- ML
mathjax: true
---
# Chapter15-DimensionalityReduction


## PCA -principal component analysis
找到一个低维的平面，来对数据进行投影，以便最小化投影误差的平方，以及最小化每个点，与投影后的对应点之间距离的平方。

![](https://ws1.sinaimg.cn/large/006tNbRwly1fxfuex4gj3j30x00ifwiq.jpg)
![](https://ws3.sinaimg.cn/large/006tNbRwly1fxfuewz5suj30wx0igdj8.jpg)
![](https://ws4.sinaimg.cn/large/006tNbRwly1fxfuews2mwj30ww0id78e.jpg)

## choose K
![](https://ws4.sinaimg.cn/large/006tNbRwly1fxfuw1li76j30ps0efn08.jpg)
![](https://ws3.sinaimg.cn/large/006tNbRwly1fxfuw1hl93j30pv0ejdk8.jpg)
![](https://ws1.sinaimg.cn/large/006tNbRwly1fxfuw1aqg3j30pr0eemz2.jpg)

## compressed representa1on
![](https://ws4.sinaimg.cn/large/006tNbRwly1fxfv3n3qvqj30ps0eetbb.jpg)

$$X_{approx} = U_{reduce}Z$$ (\\(X_{approx} \in R^{nx1}\\),\\(U_{reduce}\in R^{nxk}\\),\\(Z\in R^{kx1}\\))

## Advice for applying PCA

