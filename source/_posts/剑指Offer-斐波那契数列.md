---
title: 剑指Offer--斐波那契数列
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:48:11
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
```
class Solution:
    def Fibonacci(self, n):
        # write code here
        if n == 0:
            return 0
        elif n == 1 or n == 2:
            return 1
        else :
            a = 1
            b = 1
            for i in range(3,n):
                c = a+b
                a = b
                b = c
            return a+b
```