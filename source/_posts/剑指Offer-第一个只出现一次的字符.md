---
title: 剑指Offer--第一个只出现一次的字符
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:33:03
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---
## 题目描述
在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.
## 分析
因为要区分大小写，总共会有52个字母，相对于字符串长度可以忽略，因为维护一个长度为52的字典貌似是可行的方法，此字典中key是52个字母，value是该字母出现的index，初始设为-1。依次遍历字符串，若该字母不在dict中则说明是第一次出现，将索引index记录为它的value，若字母已经在dict中，则将该字母的value+n(n为字符串长度)。遍历完成后，在dict中，若value=-1说明该字母从来没出现过，若value>=n则说明该字母已经出现过，这两类字母不可能是结果，在剩下的字母中找出value最小的那个，就是结果。
### 已经出现的字母 value+= n ：
这样既能标记处它不符合要求，又可以反映出它和从来没出现过的字母的区别。
相当于一个节点储存了两种信息。
```python
# -*- coding:utf-8 -*-
import string
class Solution:
    def FirstNotRepeatingChar(self, s):
        # write code here
        if len(s) == 0:
            return -1
        res = {}
        for word in string.lowercase+string.uppercase:
            res[word] = -1
        for i in range(len(s)):
            c = s[i]
            if res.get(c) == -1:# 第一次出现，记录它的位置
                res[c] = i
            else: # 不是第一次出现，位置加n
                res[c]+=len(s)
        res = filter(lambda item: True if item[1]<len(s)and item[1] >=0 else False ,res.items())
        return sorted(res,key=lambda d:d[1])[0][1]
```
