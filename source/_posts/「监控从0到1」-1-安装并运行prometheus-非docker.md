---
title: '「监控从0到1」-1-安装并运行prometheus(非docker)'
author: bellick
copyright: true
top: 0
date: 2021-12-17 14:38:34
categories:
- 监控系统
tags:
mathjax:
---

# 「监控从0到1」-1-安装并运行prometheus(非docker)



## 1. 基于二进制包安装

1. 下载prometheus二进制包

```
curl -LO  prometheus-2.32.0.linux-amd64.tar.gz
```

解压

```
tar xzvf prometheus-2.32.0.linux-amd64.tar.gz
```



2. 设置配置文件

进入目录

```
cd prometheus-2.32.0.linux-amd64
```

 目录下包含默认的Prometheus配置文件promethes.yml

```
# my global config
global:
  scrape_interval:     15s # Set the scrape interval to every 15 seconds. Default is every 1 minute.
  evaluation_interval: 15s # Evaluate rules every 15 seconds. The default is every 1 minute.
  # scrape_timeout is set to the global default (10s).

# Alertmanager configuration
alerting:
  alertmanagers:
  - static_configs:
    - targets:
      # - alertmanager:9093

# Load rules once and periodically evaluate them according to the global 'evaluation_interval'.
rule_files:
  # - "first_rules.yml"
  # - "second_rules.yml"

# A scrape configuration containing exactly one endpoint to scrape:
# Here it's Prometheus itself.
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  - job_name: 'prometheus'

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
    - targets: ['localhost:9090']
```

修改`prometheus.yml` 的 `scrape_configs` 部分

```
scrape_configs:
  # The job name is added as a label `job=<job_name>` to any timeseries scraped from this config.
  
  # prometheus默认的监控项：监控prometheus的运行情况
  - job_name: "prometheus"

    # metrics_path defaults to '/metrics'
    # scheme defaults to 'http'.

    static_configs:
      - targets: ["139.198.190.25:9090"]
        labels:
                instance: prometheus

	# 增加prometheus配置项：监控node-exporter收集的系统数据
  - job_name: "linuxnode"
    static_configs:
      - targets: ["139.198.190.25:9100"]
        labels:
                instance: linuxlocalhost
```

Promtheus作为一个时间序列数据库，其采集的数据会以文件的形式存储在本地中，默认的存储路径为`data/`，因此我们需要先手动创建该目录：

```
mkdir -p data
```



3. 运行prometheus

prometheus目录下有一个可执行文件 `prometheus`,执行这个可执行文件

```
./prometheus --config.file=./prometheus.yml
```

`--config.file=./prometheus.yml` 用来指定基于刚刚设置的配置文件`./prometheus.yml`来启动prometheus



正常的情况下，你可以看到以下输出内容：

```
level=info ts=2018-10-23T14:55:14.499484Z caller=main.go:554 msg="Starting TSDB ..."
level=info ts=2018-10-23T14:55:14.499531Z caller=web.go:397 component=web msg="Start listening for connections" address=0.0.0.0:9090
level=info ts=2018-10-23T14:55:14.507999Z caller=main.go:564 msg="TSDB started"
level=info ts=2018-10-23T14:55:14.508068Z caller=main.go:624 msg="Loading configuration file" filename=prometheus.yml
level=info ts=2018-10-23T14:55:14.509509Z caller=main.go:650 msg="Completed loading of configuration file" filename=prometheus.yml
level=info ts=2018-10-23T14:55:14.509537Z caller=main.go:523 msg="Server is ready to receive web requests."
```

浏览器查看

![image-20211217145112782](http://r45qc79hv.hd-bkt.clouddn.com/uPic/image-20211217145112782.png)





## 2. 基于源码运行prometheus



```
$ mkdir -p $GOPATH/src/github.com/prometheus # 项目目录
$ cd $GOPATH/src/github.com/prometheus
$ git clone https://github.com/prometheus/prometheus.git # 下载源码
$ cd prometheus
$ make build  # 编译
$ ./prometheus --config.file=your_config.yml  # 启动prometheus
```

在编译代码 `make build`的时候发现机器没有安装`nodejs` 安装如下([ref](https://www.myfreax.com/how-to-install-node-js-on-centos-7/))

```
$ curl -sL https://rpm.nodesource.com/setup_10.x | sudo bash -
$ sudo yum install nodejs
```

> 编译代码为什么需要安装npm？
>
> 这是因为源码中包括了prometheus自带的web UI界面的前端源代码，前端代码基于nodejs开发，需要nodejs环境和npm进行编译和打包

如果`make build`的时候发现安装`npm`包特别慢，可以考虑更改为国内源, 然后再编译

```
npm set registry https://registry.npm.taobao.org/
```

