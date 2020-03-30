---
title: flask-nginx-上传大文件出错
author: bellick
copyright: true
top: 0
date: 2020-03-30 13:31:12
categories:
- flask
- flask-disk
tags:
- flask
mathjax:
---

# flask-nginx-上传大文件出错

## 问题背景

上传文件是，报413错误


413返回码的含义是，传输数据大于上限。

例如nginx／apache默认上限是1M

## 解决办法



在nginx配置文件中添加如下配置

```
client_max_body_size     1024m; //文件大小限制，默认1m

client_header_timeout    1m;

client_body_timeout      1m;

proxy_connect_timeout     60s;

proxy_read_timeout      1m;

proxy_send_timeout      1m;
```

## 相关博客

https://www.cnblogs.com/maxiaohei/p/9356368.html
