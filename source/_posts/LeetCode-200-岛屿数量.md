---
title: LeetCode-200-岛屿数量
author: bellick
copyright: true
top: 0
date: 2020-04-12 16:17:34
categories:
- Algorithms
- LeetCode
tags:
- DFS
mathjax:
---

# LeetCode-200-岛屿数量

## 题目

给定一个由 '1'（陆地）和 '0'（水）组成的的二维网格，计算岛屿的数量。一个岛被水包围，并且它是通过水平方向或垂直方向上相邻的陆地连接而成的。你可以假设网格的四个边均被水包围。

示例 1:

输入:
11110
11010
11000
00000

输出: 1
示例 2:

输入:
11000
11000
00100
00011

输出: 3

## 代码

```java

class Solution {
    public void dfs(int x, int y ,char[][] grid){
        int n = grid.length;
        int m = grid[0].length;
        grid[x][y] = '0';
        if(x-1 >= 0 && grid[x-1][y] == '1')dfs(x-1,y,grid);
        if(x+1 < n && grid[x+1][y] == '1')dfs(x+1,y,grid);
        if(y-1 >= 0 && grid[x][y-1] == '1')dfs(x,y-1,grid);
        if(y+1 < m && grid[x][y+1] == '1')dfs(x,y+1,grid);
    }
    public int numIslands(char[][] grid) {
        int n = grid.length;
        if(n == 0)return 0;
        int m = grid[0].length;
        int res = 0;
        for(int i = 0;i<n;i++){
            for(int j = 0;j<m;j++){
                if(grid[i][j] == '1'){
                    res++;
                    dfs(i,j,grid);
                }
            }
        }
        return res;
    }
} 

```

