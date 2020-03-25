---
title: 剑指Offer--1+2+3+...+n
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:29:47
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
# 题目描述
求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。
# 思路
利用逻辑与的短路特性，进行递归出口的判断

```
# -*- coding:utf-8 -*-
class Solution:
    def Sum_Solution(self, n):
        # write code here
        result = n
        temp = n > 0 and self.Sum_Solution(n-1)
        result = result+temp
        return result

```