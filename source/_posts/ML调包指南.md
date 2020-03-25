---
title: ML调包指南
author: bellick
copyright: true
top: 0
date: 2018-12-21 20:16:51
categories:
- MachineLearning
tags:
mathjax:
---
# 1. sklearn
## 1.1 SVM

```
import sklearn.svm
svc1 = sklearn.svm.LinearSVC(C=1, loss='hinge')
svc1.fit(data[['X1', 'X2']], data['y'])
svc1.score(data[['X1', 'X2']], data['y'])

data['SVM1 Confidence'] = svc1.decision_function(data[['X1', 'X2']])

LinearSVC(C=1, class_weight=None, dual=True, fit_intercept=True,
     intercept_scaling=1, loss='hinge', max_iter=1000, multi_class='ovr',
     penalty='l2', random_state=None, tol=0.0001, verbose=0)
```
