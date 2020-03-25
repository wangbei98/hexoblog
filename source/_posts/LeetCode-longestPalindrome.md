---
title: LeetCode_longestPalindrome
author: bellick
copyright: true
top: 0
date: 2019-02-27 11:48:00
categories:
- Algorithms
- LeetCode
tags:
- 回文数
- 字符串
mathjax:
---

## 题目：

给定一个字符串 s，找到 s 中最长的回文子串。你可以假设 s 的最大长度为 1000。

* 输入: "babad"
* 输出: "bab"
* 注意: "aba" 也是一个有效答案。

## 思路：中心扩展算法
事实上，只需使用恒定的空间，我们就可以在 O(n^2)的时间内解决这个问题。

我们观察到回文中心的两侧互为镜像。因此，回文可以从它的中心展开，并且只有 2n - 1个这样的中心。

你可能会问，为什么会是 2n - 1 个，而不是 n 个中心？原因在于所含字母数为偶数的回文的中心可以处于两字母之间（例如 “abba” 的中心在两个 ‘b’ 之间）。

## 实现

```
class Solution:
    def longestPalindrome(self, s: str) -> str:
        if len(s) < 1:
            return ''
        start = 0
        end = 0
        for i in range(len(s)):
            len1 = self.expendAroundCenter(s,i,i) #回文子串的中心是一个元素
            len2 = self.expendAroundCenter(s,i,i+1) # 回文子串的中心在两个元素中间
            length = max(len1,len2)
            if length > (end-start):
                start = i - int((length-1)/2)
                end = i + int(length/2)
        return s[start:end+1] # 此处要注意是 end+1 而不是end，因为切片不含最后一项
        
    # 由中心向两边扩展，返回符合要求的最长的串的长度
    def expendAroundCenter(self,s,left,right):
        L, R= left, right
        while(L>=0 and R<len(s) and s[L]==s[R]):
            L,R = L-1,R+1
        return R-L-1 # 此处注意是 R-L-1 ,因为循环停止时已经执行了 L--,R++
        
```

