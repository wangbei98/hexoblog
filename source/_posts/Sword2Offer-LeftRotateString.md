---
title: Sword2Offer-LeftRotateString
author: bellick
copyright: true
top: 0
date: 2019-04-23 13:50:10
categories:
- Algorithms
- NowCoder
tags:
- String
mathjax:
---
# 左旋转字符串
## 题目描述
汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！
## 思路
1. 相当于两个相邻数组互换

(a1,,,,am,b1,,,bn）-->（b1,,,bn,a1,,,am）

首先对全部元素原地逆置，再对前n个元素和后m个元素分别逆置

2. 把前 n 个元素组成的字符串放到 后面的子串后面 
```
# -*- coding:utf-8 -*-
class Solution:
    def LeftRotateString(self, s, n):
        # write code here
        return s[n:]+s[:n]
```