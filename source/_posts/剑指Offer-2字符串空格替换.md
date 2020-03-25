---
title: 剑指Offer--2字符串空格替换
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:49:25
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
请实现一个函数，将一个字符串中的每个空格替换成“%20”。例如，当字符串为We Are Happy.则经过替换之后的字符串为We%20Are%20Happy。
```
# -*- coding:utf-8 -*-
class Solution:
    # s 源字符串
    def replaceSpace(self, s):
        # write code here
        words = s.split(' ')
        res = "%20".join(words)
        return res
```
ac:100
python大法好