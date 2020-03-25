---
title: 剑指Offer-第一个公共节点
author: bellick
copyright: true
top: 0
date: 2019-04-23 14:39:08
categories:
- Algorithms
- NowCoder
tags:
mathjax:
---


输入两个链表，找出它们的第一个公共结点。
基本思路：
两个链表若有相同的节点，则相同节点后的所有节点都相同，所以两个链表一定是 
"Y"型的，而不可能是"X" 型的。因此如果两个链表的最后一个几点不同，则一定不会有重合的部分。
假设一个链表比另一个长k个节点，则长链表的前k个节点一定不会有我们想要的节点，所以先将长链表的指针指向k+1个节点，短链表指针指向第一个节点，同步遍历，若遇到相同的节点，则后面所有节点对应相同。
```
# -*- coding:utf-8 -*-
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None
class Solution:
    def FindFirstCommonNode(self, pHead1, pHead2):
        # write code here
        if pHead1 == None or pHead2 == None :
            return None
        len1 ,len2 = 0,0
        p1,p2 = pHead1,pHead2
        while p1 != None:
            len1 += 1
            p1 = p1.next
        while p2 != None:
            len2 += 1
            p2 = p2.next
        if p1 != p2:
            return None
        if len1 > len2:
            longlist ,shortlist = pHead1,pHead2
        else :
            longlist,shortlist = pHead2,pHead1
        for i in range(abs(len1-len2)):
            longlist = longlist.next
        while longlist!=shortlist:
            longlist,shortlist = longlist.next,shortlist.next
        return longlist
```