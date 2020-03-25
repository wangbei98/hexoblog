---
title: 剑指Offer-青蛙跳台阶
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:46:41
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。
## 分析
因为每次只能跳1或2级，想跳上第n个台阶，必须先跳到第n-1 级或者第n级
所以F(n) = F(n-1)+F(n-2)，类似于斐波那契数列
这里我犯了一个错误，考虑到在n-2 级，可以选择一次跳两级，也可以选择每次跳1级，跳两次，以为F(n) = 2*F(n-1)+F(n-2)，实际上是错误的，因为如果一次跳一级分两次跳的话，跳一级之后到n-1级就是F(n-1) 的情况，不用重复计算，所以只能选择一次跳两级。
```
class Solution:
    def jumpFloor(self, number):
        # write code here
        if number == 1 or number == 2:
            return number
        a,b = 1,2
        for i in range(number-2):
            c = a+b
            a = b
            b = c
        return c
```