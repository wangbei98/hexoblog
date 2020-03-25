---
title: LongestCommonPrefix
author: bellick
copyright: true
top: 0
date: 2019-02-27 16:30:48
categories:
- Algorithms
- LeetCode
tags:
- 最长公共前缀
mathjax: true
---

## 题目

编写一个函数来查找字符串数组中的最长公共前缀。
如果不存在公共前缀，返回空字符串 ""。

示例 1:

* 输入: ["flower","flow","flight"]
* 输出: "fl"

示例 2:

* 输入: ["dog","racecar","car"]
* 输出: ""
* 解释: 输入不存在公共前缀。

## 解法

1. 水平扫描，逐步求精
先求前两项的最长公共前缀，再将此前缀和第三项求最长公共前缀，以此类推

```
def longestCommonPrefix(self,strs:List[str])->str:
	if len(strs)==0:
		return ''
	prefix = strs[0]
	for i in range(len(strs)):
		while strs[i].find(prefix)!=0:
			prefix = prefix[:len(prefix)-1]
			if len(prefix) == 0 :
				return ''
	return prefix
```
2. 简单粗暴

竖向比较每一元素的对应位置

```
def longestCommonPrefix(self,strs:List[str])->str:
	if len(strs)==0:
		return ''
	for j in range (len(strs[0]):
		c = strs[0][j]
		for i in range(len(strs)):
			if j == len(strs[i]) or strs[i][j] != c:
				return strs[0][:j]
	return strs[0]
```

3. 分治法

全部元素的最长公共前缀 = 左半边的最长公共前缀 和 右半边的最长公共前缀 的 最长公共前缀
4. 二分法

在任意一项中（比如strs[0]） 中寻找最长公共前缀所在的下标

将这一项一分为二

* 若左边部分是所有元素的最长公共前缀，说明要找的下标在右半边
* 若左边部分不是所有元素的最长公共前缀，说明要找的下标在左半边

