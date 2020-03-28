---
title: LeetCode-440-字典序的第K小数字
author: bellick
copyright: true
top: 0
date: 2020-03-27 20:46:36
categories:
- Algorithms
- LeetCode
tags:
- 字典序
mathjax:
---

# LeetCode-440-字典序的第K小数字

440. 字典序的第K小数字
给定整数 n 和 k，找到 1 到 n 中字典序第 k 小的数字。

注意：1 ≤ k ≤ n ≤ 109。

示例 :

输入:
n: 13   k: 2

输出:
10

解释:
字典序的排列是 [1, 10, 11, 12, 13, 2, 3, 4, 5, 6, 7, 8, 9]，所以第二小的数字是 10。


## 相关题目

定义一种二叉搜索树，二叉搜索树中每个节点增加一个属性 Lsize 记录左子树节点个数加1,求第k小的关键码结点

```
BianryTreeNode<T>* findK(BinaryTreeNode<T> *root,int k){
    if(root==NULL||k<=0){return NULL}
    if(k == root->Lsize){
        return root;
    }else if(k < root->Lsize){
        return findK(root->left,k);
    }else{
        return findK(root->right,k-Lsize);
    }
}
```

## 思路

* 字典序就是一个完全10叉森林的先序遍历
* n个数字按照层序遍历放到森林中
* 写一个函数可以get到节点的Lsize（孩子数 + 1）
* 如果 已经搜索过的节点数+当前结点的Lsize > k ，说明目标结点是当前结点的孩子，向下遍历
* 如果 已经搜索过的节点数+当前结点的Lsize < k ，说明目标结点在当前结点的右边，向右遍历



## 代码

```java

class Solution {
	public int getNum(int n,int prefix) {
		//计算当前结点的孩子结点个数（包括当前结点）
		long cur = prefix;
		long next = cur+1;
		int count = 0;
		while(cur <= n) {
			count += Math.min(n+1, next) - cur;//当前这一层的孩子个数
			cur = cur * 10;
			next = next * 10;
		}
		return count;
		
	}
	public int findKthNumber(int n, int k) {
		int curCount = 1;
		int prefix = 1;
		while(curCount < k) {
			int numOfChild = getNum(n, prefix);
			if(numOfChild + curCount > k) {
				prefix = prefix*10;
				curCount++;
			}else {
				prefix++;
				curCount+=numOfChild;
			}
		}
		return prefix;
    }
}

```