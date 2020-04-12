---
title: LeetCode-322-零钱兑换
author: bellick
copyright: true
top: 0
date: 2020-04-12 15:46:04
categories:
- Algorithms
- LeetCode
tags:
- 动态规划
mathjax:
---



# LeetCode-322-零钱兑换



## 题目

给定不同面额的硬币 coins 和一个总金额 amount。编写一个函数来计算可以凑成总金额所需的最少的硬币个数。如果没有任何一种硬币组合能组成总金额，返回 -1。

示例 1:

输入: coins = [1, 2, 5], amount = 11
输出: 3 
解释: 11 = 5 + 5 + 1
示例 2:

输入: coins = [2], amount = 3
输出: -1

## 思路

自底向上的动态规划：

* temp[i] = k 表示， amount 为 i 时最少需要 k 个硬币
* 初始条件：temp[0] = 0 ， amount为0时至少需要0个硬币
* 初始条件：最多需要 amount 个硬币，所以 temp[...] 不超过 amount + 1 



## 代码



```java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int[] temps = new int[amount+1];
        Arrays.fill(temps,amount+1);
        temps[0] = 0;        
        for(int i = 1;i<= amount;i++){
            for(int coin:coins){
                if(i - coin >= 0){
                    temps[i] = Math.min(temps[i],temps[i-coin] + 1); 
                }
            }
        }
        return temps[amount] > amount ? -1:temps[amount];
    }
}
```



