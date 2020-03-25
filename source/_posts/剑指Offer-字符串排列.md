---
title: 剑指Offer--字符串排列
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:36:47
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
## 题目描述
输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。
## 输入描述:
输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

## 解法一：递归法
每一次递归负责将参数里的每一个字符轮流作为第一个字符，并和其余字符组成的序列结合构成一个排列，return这些排列的list
```
from functools import reduce
class Solution:
    def Permutation(self,ss):
        if len(ss) == 0:
            return []
        elif len(ss) ==1:
            return ss
        elif len(ss) == 2:
            if ss[0]==ss[1]:
                return [ss]
            return [ss,ss[::-1]]
        else:
            return sorted(list(set(\
            reduce(lambda x,y:x+y,\
                      map(lambda i :[ss[i] + li\
                                    for li in self.Permutation(ss[:i]+ss[i+1:])],[i for i in range(len(ss))])))\
            ))
```
## 解法二，迭代法
n个元素的排列可由前n-1个元素的排列和第n个元素的排列求出
即 P(n) = f(P(n-1))
具体关系为，对于前n-1个元素的每一种排列，将第n个元素分别插入该排列下标为0,1,2,,n-1的地方，得到对应的n种新的排列，
```
class Solution:
    def Permutation(self,ss):
        if len(ss) == 0:
            return []
        elif len(ss) ==1:
            return ss
        elif len(ss) == 2:
            if ss[0]==ss[1]:
                return [ss]
            return [ss,ss[::-1]]
        else:
            res = self.Permutation(ss[:2])
            for ele in ss[2:]:
                new_strs = []
                for to_str in res:
                    for i in range(len(to_str)+1):
                         new_strs.append(to_str[:i] + ele + to_str[i:])
                res = new_strs

            return sorted(list(set(res)))

```