---
title: 剑指Offer--把数组排成最小的数
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:30:34
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
# 题目描述
输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。
## 思路
对于每两个数num1、num2，确定它们的先后顺序是比较容易的。
基于此，重写排序算法，即可知道完整的顺序，最后重组成一个数字即可
```
# -*- coding:utf-8 -*-
import functools
class Solution:
    def compare(self,num1,num2):
        str1 = str(num1)+str(num2)
        str2 = str(num2)+str(num1)
        if int(str1) < int(str2):#  num1 应该放到前面 ，num1 更小
            return -1
        else:
            return 1
    def PrintMinNumber(self, numbers):
        if len(numbers)==0:
            return ''
        # write code here
        new_list = sorted(numbers, key=functools.cmp_to_key(self.compare))
        new_str_list = [str(i) for i in new_list]
        return int(''.join(new_str_list))

```