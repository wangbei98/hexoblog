---
title: flask-部署-supervisor出错
author: bellick
copyright: true
top: 0
date: 2020-04-07 11:40:54
categories:
- flask
- flask-disk
tags:
- flask
mathjax:
---

# flask-部署-supervisor出错

https://blog.csdn.net/hyunbar/article/details/80111947

### 运行

supervisord -c /etc/supervisor/supervisord.conf
* 出现错误
```
Starting supervisor: Error: Another program is already listening on a port that one of our HTTP servers is configured to use.  Shut this program down first before starting supervisord.
For help, use /usr/bin/supervisord -h
```

* 解决办法

* Terminal上输入

```
ps -ef | grep supervisord
```