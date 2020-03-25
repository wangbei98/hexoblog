---
title: 剑指Offer--从头到尾打印链表
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:42:29
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    # 返回从尾部到头部的列表值序列，例如[1,2,3]
    def printListFromTailToHead(self, listNode):
        # write code here
        r_list = []
        head = listNode
        while head:
            r_list.insert(0, head.val)
            head = head.next
        return r_list

```