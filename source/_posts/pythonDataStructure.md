---
title: pythonDataStructure
author: bellick
copyright: true
top: 0
date: 2018-12-19 17:52:36
categories:
- MachineLearning
- Utils
tags:
- python
- dataStructure
mathjax:
---
# ML中的数据结构
## 1. numpy
### 1. ndarray 多维数组

#### 1. 创建
ndarray 是一个通用的同构数据容器，即其中的所有元素类型相同。当创建好一个ndarray数组是，同时会在内存中存储 ndarray 的shape 和dtype， shape是 ndarray 的维度大小的元组，dtype 是解释说明 ndarray 数据类型的对象。

```
data = [1,2,3]
data = (1,2,3)
data = [[1,2,3],[4,5,6]]
arr = np.array(data)
```
#### 2. zeros

```
np.zeros(3)
np.zeros((3,4))
```
#### 3. ones
```
np.ones(3)
np.ones((3,4))
```
#### 4. arange

```
np.arange(10)
np.arange(-10,10,step=0.1)

```

#### 5. eye
### ndarray 对象属性
| 属性 | 使用说明|
|:---:|:------|
|.ndim|秩，即数据轴的个数|
|.shape|数组的维度|
|.size|元素的总个数|
|.dtype|数据类型|
|.itemsize|每个元素的字节大小|
### ndarray 数据类型
### 数组变换
#### 重塑

```
np.arange(12).reshape(3,4)
```
#### 合并

```
np.concatenate([arr1,arr2],axis=0)# 纵向
np.concatenate([arr1,arr2],axis=0)# 横向
```
#### 转置

```
# transpose 需要传入轴编号组成的元组
arr.transpose(1,0)
arr.transpose()
arr.T
```
#### 随机数

```
# 100 - 200 的随机整数
arr = np.random.randint(100,200,size=(5,4))
# 生成平均数为0 ，标准差为1的正态分布随机数，传入的参数为形状
arr = np.random.randn(1,3)
# normal 函数生成指定均值和标准差的正太分布
arr = np.random.normal(4,5,size=(#))

```
### 数组的运算
#### 数组和标量间的运算

```
arr = np.arange(3)
arr*10
>> array([10,20,30])
arr*arr
>> array([1,4,9])
```

### 矩阵运算
| # | ndarray| np.matrix| 
|:-:|:------ |:--------:|
|转置|.T|.T|
|* |	对应相乘 ，若其中有一维数组，一维数组可以向多维数组扩展|矩阵点积|
|.dot()|矩阵点积|矩阵点积|
|@| 矩阵点积 相当于.dot()|矩阵点积 相当于.dot()|
|np.multiply(m1,m2)|对应相乘,一维扩展|对应相乘，一维扩展|

### 几种数学运算
1. 向量的点乘，也叫向量的内积、数量积，对两个向量执行点乘运算，就是对这两个向量对应位一一相乘后求和的操作，结果是一个标量
2. 向量的叉乘，又叫向量积、外积、叉积，叉乘的结果是一个向量，并且与这两个向量组成的坐标平面垂直。 

## pandas