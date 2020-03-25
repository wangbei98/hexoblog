---
title: CentOS硬盘挂载
author: bellick
copyright: true
top: 0
date: 2019-06-29 18:58:35
categories:
tags:
mathjax:
---

# CentOS硬盘挂载

最近在捯饬大作业，老师让在服务器上挂载个硬盘。这咱也不会啊，幸亏给了篇[教程](https://cloud.tencent.com/developer/article/1146598)

由于对Linux理解也不大透彻，挂载硬盘是个啥意思咱也不知道。于是查到了这么一句话。

> 在linux操作系统中，挂载是一个非常重要的功能，使用非常频繁。它指将一个设备（通常是存储设备）挂接到一个已存在的目录上。这个目录可以不为空，但挂载后这个目录下以前的内容将不可用。）需要理解的是，linux操作系统将所有的设备都看作文件，它将整个计算机的资源都整合成一个大的文件目录。我们要访问存储设备中的文件，必须将文件所在的分区挂载到一个已存在的目录上，然后通过访问这个目录来访问存储设备。

大概意思就是Linux不能直接读写磁盘，只能通过把磁盘文件映射到一个目录下，才可以读写磁盘的内容。先就这么理解吧。

## 基本命令

> lsblk    查看当前系统的分区情况

![](http://ww1.sinaimg.cn/large/006tNc79ly1g4i8gkw27bj309103xt90.jpg)

> lsblk -f 

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4i8gvfnmjj30eu03xmxl.jpg)

> df-h   查看磁盘使用情况

![](http://ww4.sinaimg.cn/large/006tNc79ly1g4i8jgzcbmj30bg04d3yw.jpg)

## 挂载

需求是吧 sdc 这个硬盘挂载到一个新文件下，这里挂载到 /root/newdisk（新建这个目录）

1. 分区

> fdisk    /dev/sdc

输入 n 新增分区

输入p  --print a partition table

输入 1 ，建立一个分区

连按两次回车

输入 w 退出

* 此时 lsblk 发现 sdc 下面多了一个 sdc1

2. 格式化分区

> mkfs -t ext4 /dev/sdc1   
>
> // 文件系统推荐选 ext4

3. 挂载

> mount /dev/sdc1 /root/newdisk
>
> //将sdc1分区 挂载 到 目标挂载点

4. 设置自动挂载

设置自动挂载，可以在系统每次启动时都自动挂载此分区

> vim fstab

添加一行 

```
/dev/sdc1 		/root/newdisk 	ext4 	defaults	 0 0
```

```
:wq!
```

> mount -a //使生效

哎，这个时候就挂载好了，美滋滋。