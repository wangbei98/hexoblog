---
title: getUglyNumber
author: bellick
copyright: true
top: 0
date: 2019-01-09 15:10:59
categories:
- Algorithms
- NowCoder
tags:
- uglyNumber
mathjax:
---
# 获取前N个丑数
如果一个数的素因子只有 2，3，5 ，我们称它为丑数，
1 是第一个丑数， 要求按大小输出前N个丑数

## 思路

1. 每一个丑数都是由前面的某个丑数乘2，乘3， 或乘5得来
2. 我们当然可以遍历前面所有丑数都分别乘 2、3、5 ,取其中的最小值来得到当前的丑数
3. 一种很好的方法是选取前面若干丑数中最可能乘2产生当前丑数的数 a ，最可能乘 3 产生当前丑数的数 b， 和最可能乘 5 产生当前丑数的数 c ，取 2a,3b,5c 的最小值，就是当前丑数
4. 如何选取并在遍历中更新 a, b, c ：
5. a,b,c 为前面已经求得的丑数
6. 每次寻找丑数时用到的 a,b,c 在以后寻找丑数的过程中不能重复使用
7. 所以设定 p2, p3, p5 来记录 a,b,c 在数组中的位置，
8. 每次用 p2, p3, 或 p5 来产生一个丑数时，它们就要 +1 

```
def getUglyNumber(n):
    p2 = 0
    p3 = 0
    p5 = 0
    ans = []
    ans.insert(0,1)
    for i in range(n)[1:]:
        ans.insert(i,min(ans[p2]*2,ans[p3]*3,ans[p5]*5))
        if ans[i]==ans[p2]*2:
            p2 = p2+1
        if ans[i]==ans[p3]*3:
            p3 = p3+1
        if ans[i]==ans[p5]*5:
            p5 = p5+1
    return ans
```