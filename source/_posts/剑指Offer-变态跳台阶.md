---
title: 剑指Offer--变态跳台阶
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:45:37
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。
## 经过简单的数学分析可知，结果就是 2^(number-1)
具体分析如下：
想跳到n级，可以有如下情况：
跳1次：1种
跳2次：![](https://upload-images.jianshu.io/upload_images/9138587-66322ab6715ad050.gif?imageMogr2/auto-orient/strip)
跳i次：![](https://upload-images.jianshu.io/upload_images/9138587-8bd5349977f03efc.gif?imageMogr2/auto-orient/strip)
跳number次：
![](https://upload-images.jianshu.io/upload_images/9138587-5803aafb305caebf.gif?imageMogr2/auto-orient/strip)
所以加起来就是
![](https://upload-images.jianshu.io/upload_images/9138587-c1c754901663b0d0.gif?imageMogr2/auto-orient/strip)
```
# -*- coding:utf-8 -*-
class Solution:
    def jumpFloorII(self, number):
        # write code here
        return 2**(number-1)
```


