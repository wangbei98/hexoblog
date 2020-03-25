---
title: 剑指Offer--顺时针打印矩阵
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:34:26
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
# 顺时针打印矩阵
## 题目描述
输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.
## 思路
1. 对于方阵，可以通过顺时针遍历每一层的方式，逐层遍历，得到结果
可以想到，对于最外层，只需要顺时针遍历四条边即可，但对于里层，就不那么好处理了
此时，按照从外层到内层的方向，每遍历一行/一列,都将这一行/列从矩阵中删去，依次来保证遍历每一层时，这一层都在矩阵的最外层。
遍历过的每一行/列的删除操作，不必真的删除，只需要设定四个指针（行起始，行终止，列起始，列终止），遍历一行后，移动响应的指针即可。
这样，循环终止的条件就可以设定为：行终止-行起始 == 0 || 列终止-列起始==0
2. 而对于非方阵：
在方阵中，(行终止-行起始 == 0)，(列终止-列起始==0)一定是同时达到的，而对于非方阵，这两个结束条件一定有一个比另个一个先达到，所以此时，应该在循环中加上条件判断，让循环提前跳出。
```
# -*- coding:utf-8 -*-
class Solution:
    # matrix类型为二维列表，需要返回列表
    def printMatrix(self, matrix):
        # write code here
        row_min,col_min,row_max,col_max = 0,0,len(matrix),len(matrix[0])
        list = []
        while row_max-row_min > 0 and col_max-col_min > 0 :
            i = row_min
            for j in range(col_min,col_max):
                list.append(matrix[i][j])
            row_min += 1
            if not row_max-row_min > 0 or not col_max-col_min > 0 :
                break

            j = col_max-1
            for i in range(row_min,row_max):
                list.append(matrix[i][j])
            col_max -= 1
            if not row_max - row_min > 0 or not col_max - col_min > 0:
                break

            i = row_max-1
            for j in range(col_min,col_max)[::-1]:
                list.append(matrix[i][j])
            row_max -= 1
            if not row_max - row_min > 0 or not col_max - col_min > 0:
                break

            j = col_min
            for i in range(row_min,row_max)[::-1]:
                list.append(matrix[i][j])
            col_min += 1
            if not row_max - row_min > 0 or not col_max - col_min > 0:
                break
        return list
```

