---
title: LeetCode-42-接雨水
author: bellick
copyright: true
top: 0
date: 2020-03-27 16:14:40
categories:
- Algorithms
- LeetCode
tags:
- 动态规划
- 双指针
mathjax:
---

# LeetCode-42-接雨水

42. 接雨水
给定 n 个非负整数表示每个宽度为 1 的柱子的高度图，计算按此排列的柱子，下雨之后能接多少雨水。



上面是由数组 [0,1,0,2,1,0,1,3,2,1,2,1] 表示的高度图，在这种情况下，可以接 6 个单位的雨水（蓝色部分表示雨水）。 感谢 Marcos 贡献此图。

示例:

输入: [0,1,0,2,1,0,1,3,2,1,2,1]
输出: 6


## 思路1 -- 动态规划
* 最后结果由每个小竖条组成
* 每个小竖条的高度 = min(左边最高，右边最高) - 它的高度

* 每个节点的左边最高都可以从它左边节点推出
* 每个节点的右边最高都可以从它右边节点推出


## 代码

```java
class Solution {
    public int trap(int[] height) {
        if(height == null || height.length == 0)return 0;
		int res = 0;
		int n = height.length;
		int[] leftMax = new int[n];
		int[] rightMax = new int[n];
		
		leftMax[0] = height[0];
		rightMax[n-1] = height[n-1];
		
		for(int i = 1;i<n;i++) {
			leftMax[i] = Math.max(height[i], leftMax[i-1]);
		}
		
		for(int j = n-2;j>=0;j--) {
			rightMax[j] = Math.max(height[j], rightMax[j+1]);
		}
		
		for(int i = 1;i<n-1;i++) {
			res += Math.min(leftMax[i], rightMax[i]) - height[i];
		}
		return res;
    }
}
```


## 思路1 -- 双指针

* 左右两个指针移动 ： left, height
* 同时记录left遍历过的最大高度leftheight,right遍历过的最大高度rightheight

* 如果leftheight < rightheight，则右边肯定可以兜住，只考虑leftheight
	* left++ 现在研究left所在的位置
	* 如果 height[left] < leftheight，则左边也可以兜住，这个地方可以盛水
	* 如果 height[left] > leftheight, 则左边兜不住，换下个地方，并跟新leftheight
* 如果rightheight < leftheight，则左边肯定可以兜住，只考虑rightheight
	* right-- 现在研究right所在的位置
	* 如果 height[right] < rightheight，则右边也可以兜住，这个地方可以盛水
	* 如果 height[right] > rightheight, 则右边兜不住，换下个地方，并跟新rightheight

## 代码

```java	
class Solution {
    public int trap(int[] height) {
        if(height == null || height.length == 0)return 0;
        int n = height.length;
        int res = 0;
		int left = 0;
		int right = n-1;

		int leftheight = height[left];
		int rightheight = height[right];
		while(left < right){
			if(leftheight < rightheight){
				left++;
				if(height[left] < leftheight){
					res += (leftheight - height[left]);
				}else{
					leftheight = height[left];
				}
			}else{
				right--;
				if(height[right] < rightheight){
					res += (rightheight - height[right]);
				}else{
					rightheight = height[right];
				}
			}
		}
		return res;
    }
}
```