---
title: Vue-打包发布到服务器
author: bellick
copyright: true
top: 0
date: 2020-05-27 00:23:13
categories:
- Vue
tags:
- Vue
- 服务器
mathjax:
---


# Vue-打包发布到服务器

## 打包

```
npm run build
```

会自动将打包好的资源放到dist目录下

## 本地预览

[文档](https://cli.vuejs.org/zh/guide/deployment.html#%E6%9C%AC%E5%9C%B0%E9%A2%84%E8%A7%88)

dist 目录需要启动一个 HTTP 服务器来访问 (除非你已经将 publicPath 配置为了一个相对的值)，所以以 file:// 协议直接打开 dist/index.html 是不会工作的。在本地预览生产环境构建最简单的方式就是使用一个 Node.js 静态文件服务器，例如 serve：

```
npm install -g serve

# -s 参数的意思是将其架设在 Single-Page Application 模式下
# 这个模式会处理即将提到的路由问题
serve -s dist
```

## 部署到服务器

### vue - nginx 部署

[项目nginx部署](https://juejin.im/post/5cf0800b6fb9a07ee85c0f89)

### history 模式

[文档](https://router.vuejs.org/zh/guide/essentials/history-mode.html#%E5%90%8E%E7%AB%AF%E9%85%8D%E7%BD%AE%E4%BE%8B%E5%AD%90)

### vue路由history模式刷新页面出现404问题

vue的路由模式如果使用history，刷新会报404错误。

[参考](https://segmentfault.com/a/1190000016653688)

设置nginx配置文件

```
  server {
        listen 80;
        server_name bytescloud.cn;
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        root         /root/cloud-disk/frontend/dist;#vue项目的打包后的dist
        location / {
            try_files $uri $uri/ @router;#需要指向下面的@router否则会出现vue的路由在nginx中刷新出现404
            index  index.html index.htm;
        }
        #对应上面的@router，主要原因是路由的路径资源并不是一个真实的路径，所以无法找到具体的文件
        #因此需要rewrite到index.html中，然后交给路由在处理请求资源
        location @router {
            rewrite ^.*$ /index.html last;
        }
        #.......其他部分省略
  }
```
### Nginx 403 错误 

[](https://blog.csdn.net/xiaoxiaosasasa/article/details/75270533)

启动用户和nginx用户不一致

修改nginx用户为root

## 其他

nginx log 目录 

```
/var/log/nginx/
```

