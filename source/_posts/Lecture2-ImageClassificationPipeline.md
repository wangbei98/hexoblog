---
title: Lecture2-ImageClassificationPipeline
author: bellick
copyright: true
top: 0
date: 2018-11-30 12:28:25
categories:
- DeepLearning
- CS231n
tags:
- CS231n
- ImageClassification
mathjax: true
---

## 1. First classifier:Nearest Neighbor Classifier

![](https://ws3.sinaimg.cn/large/006tNbRwly1fxq16js2sxj30k50bc76i.jpg)

数据集：CIFAR-10,提供10个label，50,000 个训练样本，10,000个测试样本。
![](https://ws1.sinaimg.cn/large/006tNbRwly1fxq18h65wkj30k60bcafv.jpg)
K-NN 中最重要的一个部分就是 距离的计算方法，下面是曼哈顿距离的计算方法。
![](https://ws4.sinaimg.cn/large/006tNbRwly1fxq1a2d7rlj30k50bc0wp.jpg)


```
import numpy as np

class NearestNeighbor:
    def __init__(self):
        pass
    def train(self,X,y):
        '''X is NxD where each row is an example. Y is 1-dimension of size N'''
        # the nearest neighbor classifier simple remembers all the training data
        self.Xtr = X
        self.ytr = y

    def predict(self,X):
        '''X is NxD where each row is an example we wish to predict label for'''
        num_test = X.shape(0)
        # lets make sure that the output type matches the input type
        Ypred = np.zeros(num_test,dtype=self.ytr.dtype)

        # loop over all test rows
        for i in range(num_test):
            # find the nearest training image to the i'th test image
            # using the L1 distance (sum of absolute value differences)
            distances = np.sum(np.abs(self.Xtr-X[i,:]),axis=1)
            min_index = np.argmin(distances) #  get the index with smallest distance  
            Ypred[i] = self.ytr[min_index] # predict the label of the nearest example
        return Ypred
    
```
另一种距离计算方法，欧氏距离

![](https://ws1.sinaimg.cn/large/006tNbRwly1fxq2ffm7noj30th0go40z.jpg)

![](https://ws1.sinaimg.cn/large/006tNbRwly1fxq2iuqzu4j30tj0gktay.jpg)
验证方法--K折交叉验证
![](https://ws2.sinaimg.cn/large/006tNbRwly1fxq2k39sc7j30tj0gl77x.jpg)
![](https://ws3.sinaimg.cn/large/006tNbRwly1fxq2kvnuq6j30tg0gm42d.jpg)

K-NN 在图片上并不常用
![](https://ws3.sinaimg.cn/large/006tNbRwly1fxq34luvupj30tg0gigt0.jpg)

![](https://ws4.sinaimg.cn/large/006tNbRwly1fxq35wysxrj30tf0gjdja.jpg)

## 2. linear classification

