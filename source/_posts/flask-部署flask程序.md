---
title: flask-部署flask程序
author: bellick
copyright: true
top: 0
date: 2020-03-30 12:06:05
categories:
- flask
- flask-disk
tags:
- flask
mathjax:
---

# flask-部署flask程序


## gunicorn

gunicorn 是一个配置容易，性能还可以的WSGI服务器

* 安装

```
pipenv install gunicorn
```

* 使用gunicorn运行一个WSGI程序

```
gunicorn [OPTIONS] 模块名: 变量名

gunicorn -w 4 wsgi:app

gunicorn -w 4 -b 0.0.0.0:8000 wsgi:app
```



## nginx

[CES配置nginx](https://yq.aliyun.com/articles/699966)


### 配置 EPEL源

sudo yum install -y epel-release
sudo yum -y update

### 安装Nginx

sudo yum install -y nginx

默认的配置文件为：/etc/nginx/nginx.conf

自定义配置文件目录为: /etc/nginx/conf.d/

### 开启端口80和443

具体操作路径： 阿里云ECS服务器 -> 安全组 -> 配置规则 -> 安全组规则 -> 入方向 -> 添加安全组规则

端口范围： 比如你要打开80端口，这里就填写 80/80 。

优先级： 优先级可选范围为1-100，默认值为1，即最高优先级。

### 配置Nginx

配置文件结构
```python
/etc/nginx/
 |
 | -- nginx.conf  # nginx 默认配置文件
 |
 | -- conf.d  # 单独配置文件
 		|
 		| -- xxx.conf
 		| -- xxxx.conf
```

nginx默认使用 nginx.conf 里的配置，为了让nginx能够运行我们的程序，我们需要在此文件中添加一段代码（服务器信息）

但为了方便组织，我们可以把针对我们的程序的配置信息，单独放在一个conf文件里面

我们在conf.d/default.conf 里面添加我们需要的server配置，它会自动拼接到 nginx.conf文件里面



* 删除/etc/nginx/nginx.conf中的server部分代码。

```
server {
...
}
```

* 在/etc/nginx/conf.d 创建自定义配置文件default.conf

```
server {
        listen 80;
        server_name xxx.cn;
        access_log /var/log/nginx/access.log;
        error_log /var/log/nginx/error.log;

        location / {
                proxy_pass http://127.0.0.1:8000;
                proxy_redirect  off;

                proxy_set_header        Host                    $host;
                proxy_set_header        X-Real_IP               $remote_addr;
                proxy_set_header        X_Forwarded_For         $proxy_add_x_forwarded_for;
                proxy_set_header        X_Forwarded_Photo       $scheme;
        }
}

```

* 操作Nginx

1.启动 Nginx
```
systemctl start nginx
```
2.停止Nginx
```
systemctl stop nginx
```
3.重启Nginx
```
systemctl restart nginx
```
4.查看Nginx状态
```
systemctl status nginx
```
5.启用开机启动Nginx
```
systemctl enable nginx
```
6.禁用开机启动Nginx
```
systemctl disable nginx
```

## supervisor

参考 [这篇文章](https://blog.csdn.net/guolindonggld/article/details/83386920)

