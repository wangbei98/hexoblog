---
title: Centos安装Python3
author: bellick
copyright: true
top: 0
date: 2022-02-03 20:29:19
categories:
tags:
mathjax:
---

# CentOS 7 安装 Python3.7

### 1. 我们先看看现有的 python2在哪里

```shell
[root@lidan /]# whereis python
python: /usr/bin/python /usr/bin/python2.7 /usr/bin/python.bak /usr/lib/python2.7 /usr/lib64/python2.7 /etc/python /usr/include/python2.7 /usr/share/man/man1/python.1.gz
[root@lidan bin]# ll python*
lrwxrwxrwx. 1 root root    9 5月  27 2016 python2 -> python2.7
-rwxr-xr-x. 1 root root 7136 11月 20 2015 python2.7
lrwxrwxrwx. 1 root root    7 5月  27 2016 python.bak -> python2
```

### 2. 接下来我们要安装编译 Python3的相关包

```shell
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make libffi-devel
```

这里面有一个包很关键`libffi-devel`，因为只有3.7才会用到这个包，如果不安装这个包的话，在 make 阶段会出现如下的报错：

```shell
# ModuleNotFoundError: No module named '_ctypes'
```

### 3. 安装pip，因为 CentOs 是没有 pip 的。

```shell
#运行这个命令添加epel扩展源 
yum -y install epel-release 
#安装pip 
yum install python-pip
```

### 4. 可以用 python 安装一下 wget

```shell
pip install wget
```

### 5. 我们可以下载 python3.7的源码包了

```shell
wget https://www.python.org/ftp/python/3.7.0/Python-3.7.0.tgz
#解压缩
tar -zxvf Python-3.7.0.tgz

#进入解压后的目录，依次执行下面命令进行手动编译
./configure prefix=/usr/local/python3 
make && make install
```

如果最后没提示出错，就代表正确安装了，在/usr/local/目录下就会有python3目录

### 6. 添加软链接

```shell
#添加python3的软链接 
ln -s /usr/local/python3/bin/python3.7 /usr/bin/python3.7 
#添加 pip3 的软链接 
ln -s /usr/local/python3/bin/pip3.7 /usr/bin/pip3.7
#测试是否安装成功了 
python -V
```

### 7. 更改yum配置，因为其要用到python2才能执行，否则会导致yum不能正常使用（不管安装 python3的那个版本，都必须要做的）

```shell
vi /usr/bin/yum 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2 
vi /usr/libexec/urlgrabber-ext-down 
把 #! /usr/bin/python 修改为 #! /usr/bin/python2
```



# 问题 -SSL

```
Caused by SSLError("Can't connect to HTTPS URL because the SSL module is not available.
```

解决方法去python3 的安装目录下的/usr/local/python3/Python-3.6.8/Modules/Setup文件里，去掉下面四行的注释

![img](https://img-blog.csdnimg.cn/202001021454428.png)

重新编译并安装
