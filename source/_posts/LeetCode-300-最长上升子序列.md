---
title: LeetCode-300-最长上升子序列
author: bellick
copyright: true
top: 0
date: 2020-03-28 16:20:25
categories:
- Algorithms
- LeetCode
tags:
- 动态规划
mathjax:
---

# LeetCode-300-最长上升子序列

## 思路

* 记录每个位置的最长上升子序列
* 对于每个位置，扫描这个位置之前的所有位置
* 如果当前位置 > 扫描到的位置：更新当前的最长上升子序列


## 代码

```java
class Solution {
    public int lengthOfLIS(int[] nums) {
        if(nums.length == 0)return 0;
        int[] dp = new int [nums.length];
        Arrays.fill(dp, 1);
        int res = 0;
        for(int i = 0;i<nums.length;i++) {
        	for(int j = 0;j<i;j++) {
        		if(nums[i] > nums[j]) {
        			dp[i] = Math.max(dp[j] + 1, dp[i]);
        		}
        	}
        	res = Math.max(res, dp[i]);
        }
        return res;
    }
}
```
