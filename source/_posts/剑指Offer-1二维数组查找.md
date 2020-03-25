---
title: 剑指Offer--1二维数组查找
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:50:53
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 分析
[[1,2,8,9],
[2,4,9,12],
[4,7,10,13],
[6,8,11,15]]
形如上面的二维数组，可以发现最右上角的点最为特殊，如果它大于target，则最后一列一定全大于target，故不用考虑，如果它小于target，则第一行全部小于target，也不用考虑
因此可以想到，每次考虑矩阵的右上角元素，如果大于target，则删除最后一列，小于target则删除第一行，等于则返回True
这里“删除”操作并不需要实际执行，移动下标 i, j 即可

```
class Solution:
    # array 二维列表
    def Find(self, target, array):
        # write code here
        m = len(array)
        n = len(array[0])
        i,j = 0,n-1
        while(i<m and j>=0):
            if array[i][j]< target:
                i+=1
            elif array[i][j] > target:
                j-=1
            else:
                return True
        return False
```
ac:100%