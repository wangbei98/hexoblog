---
title: Redis入门
author: bellick
copyright: true
top: 0
date: 2021-03-16 20:07:25
categories:
- Redis
tags:
mathjax:
---

# Redis 入门



## 安装

> $ git clone https://github.com/redis/redis.git
>
> $ cd redis
>
> $ make -j
>
> $ ./src/redis-server
>
> $ ./src/redis-cli // 在另一个终端连接



## Java 客户端

```xml
<dependency>
    <groupId>redis.clients</groupId>
    <artifactId>jedis</artifactId>
    <version>3.5.1</version>
</dependency>
```



```java
Jedis jedis = new Jedis("127.0.0.1",6379);
// jedis 对象可以使用redis的所有命令
System.out.println(jedis.ping());
System.out.println(jedis.flushDB());
jedis.close();
```





## redis 配置

redis.conf文件

* Bind 是否允许被其他机器访问

* append only [no, yes] 是否打开AOF
* appendfsync [everysec, always, no] AOF刷新策略





## 选型

* 单副本：会有主动的HA，但新的节点是空的