---
title: 剑指Offer--1出现的个数
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:52:00
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---

# 整数中1出现的次数（从1到n整数中1出现的次数）

## 题目描述
求出1~13的整数中1出现的次数,并算出100~1300的整数中1出现的次数？为此他特别数了一下1~13中包含1的数字有1、10、11、12、13因此共出现6次,但是对于后面问题他就没辙了。ACMer希望你们帮帮他,并把问题更加普遍化,可以很快的求出任意非负整数区间中1出现的次数（从1 到 n 中1出现的次数）。
## 思路


```
# -*- coding:utf-8 -*-
class Solution:
    def NumberOf1Between1AndN_Solution(self, n):
        # write code here
        i = 1
        count = 0
        while i <= n:
            a ,b = int(n/i),n%i
            count += int((a+8)/10) * i + ((b+1) if a%10==1 else 0)
            i *= 10
        return count
```