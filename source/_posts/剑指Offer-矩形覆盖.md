---
title: 剑指Offer--矩形覆盖
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:44:58
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---

我们可以用2*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2*1的小矩形无重叠地覆盖一个2*n的大矩形，总共有多少种方法？
```
# -*- coding:utf-8 -*-
class Solution:
    def rectCover(self, number):
        # write code here
        if number == 0 or number ==1 or number ==2:
            return number
        else:
            a,b = 1,2
            for i in range(number-2):
                c = a+b 
                a = b
                b = c
            return c
```

