title: chapter13. SVM
author: bellick
tags: []
categories:
  - MachineLearning
  - MLbyAndrewNg
date: 2018-11-14 10:36:00
mathjax: true
---

# SVM support vector machine
## 1. 支持向量机的数学定义
![](https://ws3.sinaimg.cn/large/006tNbRwly1fx7e37hy4sj30ld09t0tv.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7e54lpvwj30kq058dha.jpg)

![](https://ws4.sinaimg.cn/large/006tNbRwly1fx7eadphfyj30ke06rq4v.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7ebb723aj30kd03zjsc.jpg)
![](https://ws3.sinaimg.cn/large/006tNbRwly1fx7ej70nzoj30io02k74s.jpg)
## 2. Large Margin 
![](https://ws1.sinaimg.cn/large/006tNbRwly1fx7eo2ipplj30jq0axtbx.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7es648kfj30jv0aqtb3.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7ew2hwu2j30k60avtaz.jpg)

当C较小时，SVM会适当地忽略一些噪点
## 3.the mathematics behind large margin classification

![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7gno9ghzj30kv0bfwk1.jpg)
## 4. kernels1 
![](https://ws4.sinaimg.cn/large/006tNbRwly1fx7h9ho1gxj30mr0csn0c.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7guaz6m9j30k40b6wi1.jpg)
![](https://ws3.sinaimg.cn/large/006tNbRwly1fx7h02hz7hj30kk0ayq7f.jpg)


## 5. kernels2
#### Gaussian kernel
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7hckf9vfj30lm08vabu.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7hjhagdgj30mu0cugp0.jpg)
![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7ht83x2jj30mt0cyju7.jpg)
在有核SVM中，测试点到每一个被当做landmark的样本点的高斯kernel特征都会当做参数，所以有多少landmark，就会有多少个 \\(\theta\\),故此时 m=n 。

\\(\sum_j\theta\_j^2  = \theta^T\theta\\)

改成

\\(\sum_j\theta\_j^2  = \theta^TM\theta\\)

可以在样本量很大时优化计算。

![](https://ws2.sinaimg.cn/large/006tNbRwly1fx7i25u2ioj30ms0cuq55.jpg)
## 6. Using an SVM
![](https://ws1.sinaimg.cn/large/006tNbRwly1fx7iuewh3wj30mt0cvgon.jpg)