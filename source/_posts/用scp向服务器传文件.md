---
title: 用scp向服务器传文件
author: bellick
copyright: true
top: 0
date: 2019-07-02 16:11:24
categories:
tags:
mathjax:
---

# 用scp传文件

为了搭建Hive环境，需要向服务器传一些配置文件，最初想用 filezilla ，但经过几个小时的尝试，看了网上几篇教程，还是连接不上。



问了一下学长，学长说服务器没装ftp，端口也只开了三个，推荐我使用scp。

那SCP又是个啥呢，咱也不敢问，Google一下。

嗯，这篇文章说的还挺清楚。

利用scp传输文件
  1、从服务器上下载文件
  scp username@servername:/path/filename
  例如scp codinglog@192.168.0.101:/home/kimi/test.txt  把192.168.0.101上的/home/kimi/test.txt
  的文件下载到当前目录
  2、上传本地文件到服务器
  scp /path/filename username@servername:/path  
  例如scp /var/www/test.php  codinglog@192.168.0.101:/var/www/  把本机/var/www/目录下的test.php文件
  上传到192.168.0.101这台服务器上的/var/www/目录中

  3、从服务器下载整个目录
      scp -r username@servername:remote_dir/ local_dir/
    例如:scp -r codinglog@192.168.0.101 /home/kimi/test  /home/kimi/  
  4、上传目录到服务器
      scp  -r local_dir username@servername:remote_dir
      例如：
      scp -r test      codinglog@192.168.0.101:/var/www/   把当前目录下的test目录上传到服务器的/var/www/ 目录

作者：ssihc0 
来源：CSDN 
原文：https://blog.csdn.net/ssihc0/article/details/7544573 
版权声明：本文为博主原创文章，转载请附上博文链接！