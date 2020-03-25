---
title: 剑指Offer--主元素
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:40:34
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
## 找出主元素（某元素的个数超过数组长度的一半）
主元素的特点： 如果一个元素的个数一定超过后面不等于它的元素的个数
将第一个遇到的整数num保存在c中，记录num出现的次数为1， 若遇到的下一个元素仍是num，则计数加1，若不等于num，则计数减1。当计数等于0时，将遇到的下一个元素保存到c中，计数重新记为1，开始新一轮计数，直到扫描完所有的元素
```
# -*- coding:utf-8 -*-

class Solution:
    def MoreThanHalfNum_Solution(self, numbers):
        # write code here
        temp = numbers[0]
        count = 1
        for n in numbers[1:]:
            if n == temp:# 相同
                count+=1
            else: # 不同
                if count == 0:
                    temp = n
                    count = 1
                else:
                    count -=1
        # 此时找到的temp是可疑值，需要进一步验证
        if count > 0:# 验证
            count = 0
            for n in numbers:
                if n == temp:
                    count += 1
            if count > len(numbers)/2:
                return temp
            else:
                return 0
        return 0
if __name__ =='__main__':
    li = [4,2,1,4,2,4]
    s = Solution()
    print(s.MoreThanHalfNum_Solution(li))
```
### 方法二 暴力求解
```
    class Solution:
    def MoreThanHalfNum_Solution(self, numbers):
        # write code here
        if len(numbers) == 0:
            return 0
        n = len(numbers) / 2
        for i in list(set(numbers)):
            if numbers.count(i) > n:
                return i
        return 0
```