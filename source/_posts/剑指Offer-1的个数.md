---
title: 剑指Offer--1的个数
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:43:58
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
输入一个整数，输出该数二进制表示中1的个数。其中负数用补码表示。
整数 i 与这个整数减 1 的值 i - 1，按位求与，如此可以将整数的二进制表示中最低位的 1变成0 。
二进制中有几个1，便需要操作几次。
```
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1(self, n):
        # write code here
        # Python 中，整数是没有范围限制的，不会溢出（overflow），需要人工限定范围。
        INT_BITS = 32
        MAX_INT = (1 << (INT_BITS - 1)) - 1
        count = 0
        while n:
            if n < - MAX_INT- 1 or n > MAX_INT:
                break
            n = n & (n - 1)
            count += 1
        return count
```