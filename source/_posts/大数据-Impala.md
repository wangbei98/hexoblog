---
title: 大数据-Impala
author: bellick
copyright: true
top: 0
date: 2019-07-05 17:37:28
categories:
tags:
mathjax:
---

Impala

新坑待填

https://blog.csdn.net/lukabruce/article/details/82783167

https://blog.csdn.net/qq_41792743/article/details/87979146





>  service impala-state-store restart --kudu_master_hosts=localhost:7051
>
> service impala-catalog restart --kudu_master_hosts=localhost:7051
>
> service impala-server restart --kudu_master_hosts=localhost:7051





Impala-shell 安装问题

> rpm -ivh impala-shell-2.9.0+cdh5.12.1+0-1.cdh5.12.1.p0.3.el7.x86_64.rpm

报错：

>error: Failed dependencies: python-setuptools is needed

解决办法：

> yum install -y python-setuptools

重复安装

>rpm -ivh impala-shell-2.9.0+cdh5.12.1+0-1.cdh5.12.1.p0.3.el7.x86_64.rpm

Complete！



impala 连接问题

> Not connected 



启动Hive元数据

>hive --service metastore &

![](http://ww3.sinaimg.cn/large/006tNc79ly1g4p4ajus4rj30kj0a20tg.jpg))

Linux 查看端口占用情况可以使用 **lsof** 和 **netstat** 命令。

------

## lsof

lsof(list open files)是一个列出当前系统打开文件的工具。

lsof 查看端口占用语法格式：

```
lsof -i:端口号
```

### 实例

查看服务器 8000 端口的占用情况：

```
# lsof -i:8000
COMMAND   PID USER   
```