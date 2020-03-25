---
title: 剑指Offer--和为S的两个数字
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:31:41
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
## 题目描述
输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。
输出描述:
对应每个测试案例，输出两个数，小的先输出。
## 分析： 此题可以采用LeetCode上TwoSum那一题的解法，用dict储存每一个元素期望出现的complement ，但这样“增序数组”这一条件就浪费了。
和为特定值的两个数的乘积有以下特点：两数分布越集中，乘积越大，具体可参考函数 f(x) = (S-x)x
因此考虑从两侧遍历数组，找到的第一组符合条件的数对即是最小的。
```
# -*- coding:utf-8 -*-
class Solution:
    def FindNumbersWithSum(self, array, tsum):
        # write code here
        if len(array) == 0:
            return []
        i,j = 0,len(array)-1
        while i != j:
            if array[i] + array[j] == tsum:
                return array[i],array[j]
            elif array[i] + array[j] < tsum:
                i += 1
            else:
                j -= 1
        return []

s = Solution()
print(s.FindNumbersWithSum([1,2,4,7,11,16],10))

```