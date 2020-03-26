---
title: LeetCode-93-复原IP地址
author: bellick
copyright: true
top: 0
date: 2020-03-26 20:25:35
categories:
- Algorithms
- LeetCode
tags:
mathjax:
---

# 93. 复原IP地址
给定一个只包含数字的字符串，复原它并返回所有可能的 IP 地址格式。

示例:

输入: "25525511135"
输出: ["255.255.11.135", "255.255.111.35"]


## 回溯算法

### 回溯思想

* 如果满足条件，则加入结果集
* 如果不满足条件，for循环里的递归，递归调用之后，撤销选择

### 基本框架

```java
	backtrack(int first){ //first 表示当前从first开始扫描
		if(满足条件){

		}
		for(遍历所有可能的current){//current 从first开始遍历
			// 试探
			backtrack(current+1)
			// 撤销试探
		}
	}
```


```java
class Solution {
    LinkedList<String> curSegments = new LinkedList<>();
	ArrayList<String> output = new ArrayList<>();
	String s;
	int n;
	public boolean isValid(String seg) {
		int len = seg.length();
		if(len > 3)return false;
		return (seg.charAt(0) != '0') ? Integer.valueOf(seg) <= 255 : len==1;
	}
	public void backtrack(int first,int restDots) {
		if(restDots==0 && isValid(s.substring(first))) {
			curSegments.add(s.substring(first));
			output.add(String.join(".", curSegments));
			curSegments.removeLast();
		}
		int maxPos = Math.min(n-1, first+3);
		for(int cur = first;cur<maxPos;cur++) {
			String curSeg = s.substring(first,cur+1);
			if(isValid(curSeg)) {
				curSegments.add(curSeg);
				backtrack(cur+1, restDots-1);
				curSegments.removeLast();
			}
		}
	}
	public List<String> restoreIpAddresses(String s) {
        this.s = s;
        this.n = s.length();
        if(this.n > 12){
            return output;
        }
        backtrack(0, 3);
        return output;
    }
}
```
