---
title: Python复制、深拷贝、浅拷贝
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:22:47
categories:
- Notebook
- Python
tags:
- copy
- python
mathjax:
---
alist=[1,2,3,["a","b"]]

## 1. 直接赋值,默认浅拷贝传递对象的引用而已,原始列表改变，被赋值的b也会做相同的改变
```
>>> b=alist
>>> print b
[1, 2, 3, ['a', 'b']]
>>> alist.append(5)
>>> print alist;print b
[1, 2, 3, ['a', 'b'], 5]
[1, 2, 3, ['a', 'b'], 5]
```
## 3.copy浅拷贝，没有拷贝子对象，所以原始数据改变，子对象会改变
```
>>> import copy
>>> c=copy.copy(alist)
>>> print alist;print c
[1, 2, 3, ['a', 'b']]
[1, 2, 3, ['a', 'b']]
>>> alist.append(5)
>>> print alist;print c
[1, 2, 3, ['a', 'b'], 5]
[1, 2, 3, ['a', 'b']]
>>> alist[3]
['a', 'b']
>>> alist[3].append('cccc')
>>> print alist;print c
[1, 2, 3, ['a', 'b', 'cccc'], 5]
[1, 2, 3, ['a', 'b', 'cccc']] 里面的子对象被改变了
```