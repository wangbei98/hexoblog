---
title: Formula
author: bellick
copyright: true
top: 0
tags:
  - markdown
  - formula
categories:
  - others
  - markdown
date: 2018-11-07 12:11:00
mathjax: true
---

# 在markdown中插入数学公式
## 方法一：使用Google Chart的服务器

	<img src="http://chart.googleapis.com/chart?cht=tx&chl= 在此插入Latex公式" style="border:none;">

<img src="http://chart.googleapis.com/chart?cht=tx&chl=\Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}" style="border:none;">

##方法二：使用forkosh服务器
forkosh上提供了关于Latex公式的一份简短而很有用的帮助，参考[1]和[3].

使用forkosh插入公式的方法是

	<img src="http://www.forkosh.com/mathtex.cgi? 在此处插入Latex公式">
	
给个例子，

<img src="http://www.forkosh.com/mathtex.cgi? \Large x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}">

### 方法三：使用MathJax引擎
在markdown文件中添加以下脚本

	<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>
<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=default"></script>

然后，再使用Tex写公式。$$公式$$表示行间公式，本来Tex中使用\(公式\)表示行内公式，但因为Markdown中\是转义字符，所以在Markdown中输入行内公式使用\\(公式\\)，如下代码：

行间公式$$x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}$$
行内公式 \\(x=\frac{-b\pm\sqrt{b^2-4ac}}{2a}\\)
### LETEX语法
#### 1.2.1 上、下标
先看结果再总结语法吧。

	$$x_1$$
	
	$$x_1^2$$
	
	$$x^2_1$$
	
	$$x_{22}^{(n)}$$
	
	$${}^* x^*$$
	
	$$x_{balabala}^{bala}$$
	
	$$x_1$$
	
$$x_1$$

$$x_1^2$$

$$x^2_1$$

$$x_{22}^{(n)}$$

$${}^* x^*$$

$$x_{balabala}^{bala}$$

可以看到 x 元素的上标通过 ^ 符号后接的内容体现，下表通过 _ 符号后接的内容体现，多于一位是要加 {} 包裹的。
#### 1.2.2 分式
	$$\frac{x+y}{2}$$
	
	$$\frac{1}{1+\frac{1}{2}}$$
$$\frac{x+y}{2}$$

$$\frac{1}{1+\frac{1}{2}}$$
