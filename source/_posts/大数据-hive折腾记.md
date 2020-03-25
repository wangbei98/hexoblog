---
title: 大数据-hive折腾记
author: bellick
copyright: true
top: 0
date: 2019-07-02 18:22:19
categories:
tags:
mathjax:
---



一、安装 **MySQL**

1. 上传MySQL在线安装源的配置文件

用WinSCP（root账号连接）CentOS服务器

将mysql-community.repo 文件上传到 /etc/yum.repos.d/ 目录

将RPM-GPG-KEY-mysql 文件上传到 /etc/pki/rpm-gpg/ 目录

 

2. 更新yum源并安装mysql server（默认同时会安装mysql client）

> yum repolist

> yum install mysql-server

 

3. 查看MySQL各组件是否成功安装

> rpm -qa | grep mysql

![img](http://ww1.sinaimg.cn/large/006tNc79ly1g4lo4au6h4j308j01ngm8.jpg) 

 

 

二、配置**MySQL**

1. 启动MySQL Server并查看其状态

> systemctl start mysqld

> systemctl status mysqld

![img](http://ww4.sinaimg.cn/large/006tNc79ly1g4lo815xcsj30dz028gmn.jpg)、

2. 查看MySQL版本

> mysql -V

![img](http://ww2.sinaimg.cn/large/006tNc79ly1g4lo83tyhbj30dz00mweq.jpg) 

 

3. 连接MySQL，默认root密码为空

> mysql -u root   (这个命令不好用，用 mysql -u root -p )

> mysql> s

这里如果使用 > myswl -u root 会报以下错误

> ERROR 1044 (42000): Access denied for user ''@'localhost' to database 'mysql' 

4. 查看数据库

> mysql> show databases; （注意：必须以分号结尾，否则会出现续行输入符“>”）

 

5. 创建hive元数据数据库（metastore）

> mysql> create database hive; 

![img](http://ww4.sinaimg.cn/large/006tNc79ly1g4lo9qcxrpj308o04z0tu.jpg) 

 

6. 创建用户hive，密码是123456

> mysql> CREATE USER 'hive'@'%' IDENTIFIED BY '123456';

注意：删除用户是DROP USER命令 

 

7. 授权用户hadoop拥有数据库hive的所有权限

mysql> GRANT ALL PRIVILEGES ON hive.* TO 'hive'@'%' WITH GRANT OPTION;

 

8. 查看新建的MySQL用户（数据库名：mysql，表名：user）

> mysql> select host,user,password from mysql.user;

![img](http://ww1.sinaimg.cn/large/006tNc79ly1g4lo9tbz18j30dz04imyu.jpg) 

 

9. 删除空用户记录，如果没做这一步，新建的hive用户将无法登录，后续无法启动hive客户端

> mysql> delete from mysql.user where user='';

 

10. 刷新系统授权表（不用重启mysql服务）

> mysql> flush privileges; 

 

11. 测试hive用户登录

> mysql -u hive -p

> Enter password：123456



**三、安装和配置hive**

1. 下载hive

> Wget https://mirrors.tuna.tsinghua.edu.cn/apache/hive/hive-2.3.5/apache-hive-2.3.5-bin.tar.gz

2. 解压hive-1.1.0-cdh5.12.1.tar.gz到/home/hadoop

> $ tar zxvf apache-hive-2.3.5-bin.tar.gz

 

3. 在.bash_profile文件中添加hive环境变量

> export HIVE_HOME=/home/hadoop/hive-1.1.0-cdh5.12.1

> export PATH=$HIVE_HOME/bin:$PATH

4. 使上述设置生效

   > $ source .bash_profile

 

5. 编辑$HIVE_HOME/conf/hive-env.sh文件，在末尾添加HADOOP_HOME变量

> cd $HIVE_HOME/conf

> cp hive-env.sh.template hive-env.sh	（默认不存在，可从模板文件复制）

> vi hive-env.sh

> HADOOP_HOME=/root/Hadoop/hadoop-2.8.5

 

6. 新建$HIVE_HOME/conf/hive-site.xml文件

```xml
<?xml version="1.0" encoding="UTF-8" standalone="no"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<configuration>
        <property>
                <name>javax.jdo.option.ConnectionDriverName</name>
                <value>com.mysql.jdbc.Driver</value>
        </property>
        <property>
                <name>javax.jdo.option.ConnectionURL</name>
                <value>jdbc:mysql://localhost:3306/hive</value>
        </property>
        <property>
                <name>javax.jdo.option.ConnectionUserName</name>
                <value>hive</value>
        </property>
        <property>
                <name>javax.jdo.option.ConnectionPassword</name>
                <value>123456</value>
        </property>

		<property>
				<name>hive.metastore.warehouse.dir</name>
				<value>/hive/warehouse</value>
		</property>
		<property>
				<name>hive.exec.scratchdir</name>
				<value>/hive/tmp </value>
		</property>
        <property>
                <name>hive.metastore.schema.verification</name>
                <value>false</value>
        </property>
</configuration>
```



 

7. 在HDFS上创建数据仓库目录（用于存放hive数据文件）和临时目录

> hdfs dfs -mkdir -p /hive/warehouse /hive/tmp

 

8. 下载mysql连接驱动，下载地址：https://dev.mysql.com/downloads/connector/j/

![img](file:////var/folders/nm/nfxnvn057nq5rsjjdhz11rsw0000gn/T/com.kingsoft.wpsoffice.mac/wps-bellick/ksohtml/wpsJYMEDH.png) 

​	下载文件(.tar.gz)解压后，将其中的mysql-connector-java-8.0.13.jar文件上传到 $HIVE_HOME/lib目录下

 

9. 启动hive

> hive

 

10. 查看hive数据库 （注意：命令以分号结尾）

> hive> show databases;

![img](file:////var/folders/nm/nfxnvn057nq5rsjjdhz11rsw0000gn/T/com.kingsoft.wpsoffice.mac/wps-bellick/ksohtml/wpsY2lKug.jpg) 

default是默认数据库

11. 退出hive

> hive> quit;

### Hive> Show databases; 报错

> hive> show databases;
> FAILED: SemanticException org.apache.hadoop.hive.ql.metadata.HiveException: org.apache.hadoop.hive.ql.metadata.HiveException: MetaException(message:Hive metastore database is not initialized. Please use schematool (e.g. ./schematool -initSchema -dbType ...) to create the schema. If needed, don't forget to include the option to auto-create the underlying database in your JDBC connection string (e.g. ?createDatabaseIfNotExist=true for mysql))



在HIVE_HOME/conf/hive-site.xml 中添加如下配置

```
<property>
<name>datanucleus.schema.autoCreateAll</name>
<value>true</value>
</property>
```

