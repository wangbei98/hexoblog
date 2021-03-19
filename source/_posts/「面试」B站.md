---
title: 「面试」B站
author: bellick
copyright: true
top: 0
date: 2021-03-18 18:12:48
categories:
- 面试
tags:
mathjax:
---

# 「面试」B站



## 一面

- 20分钟项目

- Hadoop spark的使用

- 计算100万个数的平均数怎么实现

- 框架

- RESTfulAPI

- SpringBoot版本

- 内存模型

- 堆内存

- 垃圾回收算法

- 垃圾回收器

- volatile 作用、原理

- 单例模式

- CAS原理

- TCP三次握手、四次挥手、close_wait

- TCP优化策略/优化算法

- concurantHashMap实现

- countdownLatch 和 CyclicBarrier

  CountDownLatch 是计数器，只能使用一次，而 CyclicBarrier 的计数器提供 reset 功能，可以多次使用。但是我不那么认为它们之间的区别仅仅就是这么简单的一点。我们来从 jdk 作者设计的目的来看，javadoc 是这么描述它们的：

  > CountDownLatch: A synchronization aid that allows one or more threads to wait until a set of operations being performed in other threads completes.(CountDownLatch: 一个或者多个线程，等待其他多个线程完成某件事情之后才能执行；) CyclicBarrier : A synchronization aid that allows a set of threads to all wait for each other to reach a common barrier point.(CyclicBarrier : 多个线程互相等待，直到到达同一个同步点，再继续一起执行。)

  对于 CountDownLatch 来说，重点是“一个线程（多个线程）等待”，而其他的 N 个线程在完成“某件事情”之后，可以终止，也可以等待。而对于 CyclicBarrier，重点是多个线程，在任意一个线程没有完成，所有的线程都必须等待。

  CountDownLatch 是计数器，线程完成一个记录一个，只不过计数不是递增而是递减，而 CyclicBarrier 更像是一个阀门，需要所有线程都到达，阀门才能打开，然后继续执行。

- 对象头中锁字段

- 构造器模式
- 多大算大对象
- 写入操作，高并发，有序的KV，用什么数据结构
  - 跳表
- B/B+区别
- CountDownLatch 和 CyclicBarrier
- Sych
- volatile 关键字
  - i++的时候
- AQS
- 公平锁和非公平锁的区别

## 二面

- HashMap 存储
  - put过程
  - concurrentHashMap 怎么加锁的
- AVL 和 红黑树
- 跳表：
  - 节点有冗余吗
- Streaming 消费 Kafka 的两种模式
- Mysql
  - 索引
  - 查询过程
- GC 算法
  - CMS：标记清除
  - CMS 具体过程
  - G1 具体过程
- 内存泄露的原因



## 三面

* 项目
  * 项目中怎么优化查询速度的
* HashMap优化
  * 没有remove只有get(),put(),containsKey() 的场景应该如何优化
* 数组
  * 基本类型和对象数组的区别
* ArrayList 和 LinkedList
* 