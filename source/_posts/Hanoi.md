title: Hanoi
author: bellick
tags:
  - hanoi
  - python
categories:
  - Algorithms
  - ''
date: 2018-11-07 12:42:00
mathjax: true
---

**汉诺塔**：汉诺塔（又称河内塔）问题是源于印度一个古老传说的益智玩具。大梵天创造世界的时候做了三根金刚石柱子，在一根柱子上从下往上按照大小顺序摞着64片黄金圆盘。大梵天命令婆罗门把圆盘从下面开始按大小顺序重新摆放在另一根柱子上。**并且规定，在小圆盘上不能放大圆盘，在三根柱子之间一次只能移动一个圆盘**。
## 分析：
#### 1. 当只有一个圆盘时，只需要将 A 上面的圆盘移动到 C 上即可

![image](http://upload-images.jianshu.io/upload_images/9138587-38801d4faba94d91.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

#### 2. 当有两个圆盘时，就需要借助 B:
	
![image](http://upload-images.jianshu.io/upload_images/9138587-040fc26e3233cdf2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

先将最上面的圆盘移动到 B上
![image](http://upload-images.jianshu.io/upload_images/9138587-4df9fbbbd8579225.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
再将 A 上的圆盘移动到 C 上
![image](http://upload-images.jianshu.io/upload_images/9138587-0e3a53f9d02511ae.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
最后将 B 上的圆盘移动到 C 上
![image](http://upload-images.jianshu.io/upload_images/9138587-536e441dda640b04.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
这样我们就完成了将两个圆盘从A移动到C

##### 需要注意的是：上述过程中最重要的思想是借助一个空柱子B

#### 3. n个圆盘从A移动到C
当有 n 个圆盘时，我们可以将上面的 n-1 个圆盘看成一个整体，这样问题又回到了两个圆盘的情况，现在我们只需要实现将上面 n-1 个圆盘组成的整体从A移动到B，再从B移动到C。
![image](http://upload-images.jianshu.io/upload_images/9138587-bfa583e3c0a84823.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image](http://upload-images.jianshu.io/upload_images/9138587-8e343417d1c47e9a.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image](http://upload-images.jianshu.io/upload_images/9138587-a0716957b7562407.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
![image](http://upload-images.jianshu.io/upload_images/9138587-9e01bd28ca5834c2.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

我们注意到:将 n-1 个圆盘从A移动到B，从B移动到C 和将 n 个圆盘从
A移动到C是同一类问题。

因此可以构造出如下函数 hanoi(n,a,b,c),表示 n 个圆盘 借助 B从A移动到C

那么将 n-1 个圆盘从A移动到B 就可以用 hanoi(n-1,a,c,b)实现；

将n-1 个圆盘B移动到C 就可以用 hanoi(n-1,b,a,c)来实现

至此，此问题的递归实现思路渐渐清晰起来，接下来只需要用代码实现了。

## 代码实现

	# -*- coding: utf-8 -*-
	def move(n,a,b,c):
	    if n==1:
	        print (a+'-->'+c)
	    else:
	        move(n-1,a,c,b)
	        print(a+'-->'+c)
	        move(n-1,b,a,c)
	if __name__ == '__main__':
	    move(3,'A','B','C')