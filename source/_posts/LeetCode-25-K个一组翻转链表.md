---
title: LeetCode-25-K个一组翻转链表
author: bellick
copyright: true
top: 0
date: 2020-03-27 15:09:29
categories:
- Algorithms
- LeetCode
tags:
- 链表
mathjax:
---

# LeetCode-25-K个一组翻转链表

## 题目
25. K 个一组翻转链表
给你一个链表，每 k 个节点一组进行翻转，请你返回翻转后的链表。

k 是一个正整数，它的值小于或等于链表的长度。

如果节点总数不是 k 的整数倍，那么请将最后剩余的节点保持原有顺序。

 

示例：

给你这个链表：1->2->3->4->5

当 k = 2 时，应当返回: 2->1->4->3->5

当 k = 3 时，应当返回: 3->2->1->4->5

 

说明：

你的算法只能使用常数的额外空间。
你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换。

## 思路

 * 每次反转 k 个节点
 * 翻转之前找到这一段链表的前驱节点，最后一个节点
 * 翻转这段链表
 * 把这段链表接回去
 * 继续找下一段

 * 如果最后一段不够k个节点，则这一段又多长算多长


```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode(int x) { val = x; }
 * }
 */
class Solution {
    public ListNode reverseKGroup(ListNode head, int k) {
		if(k < 2) {
			return head;
		}
		ListNode dummy = new ListNode(0);
		dummy.next = head;
		
		ListNode pre = dummy;
		ListNode end = dummy;
		
		while(end.next!=null) {
			for(int i = 0;i<k & end!=null;i++)end = end.next;
			if(end == null)break;//提前结束
			ListNode curStart = pre.next;
			ListNode nextStart = end.next;
			end.next = null;//断开
			pre.next = reverse(curStart);
			curStart.next = nextStart;
			
			pre = curStart;
			end = pre;
		}
		return dummy.next;
    }
    //翻转不带头节点的链表，返回翻转后的首节点
	public ListNode reverse(ListNode head) {
		ListNode pre = head;
		ListNode cur = head.next;
		head.next = null;
		while(cur!=null) {
			ListNode next = cur.next;
			cur.next = pre;
			pre = cur;
			cur = next;
		}
		return pre;
	}
}

```