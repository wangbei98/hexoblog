---
title: 「监控从0到1」-3-开发一个简单到exporter
author: bellick
copyright: true
top: 0
date: 2021-12-21 20:12:35
categories:
- 监控系统
tags:
mathjax:
---

# 「监控从0到1」-3-开发一个简单到exporter

## demo

新建一个项目，并下载需要的包

```
mkdir my_first_exporter
cd my_first_exporter
go mod init
go get github.com/prometheus/client_golang
```



```go
package main

import (
	"flag"
	"log"
	"net/http"

	"github.com/prometheus/client_golang/prometheus/promhttp"
)

var addr = flag.String("listen-address", ":8080", "The address to listen on for HTTP requests.")

func main() {
	flag.Parse()
	http.Handle("/metrics", promhttp.Handler())
	log.Fatal(http.ListenAndServe(*addr, nil))
}
```

![image-20211221201403744](http://r45qc79hv.hd-bkt.clouddn.com/uPic/image-20211221201403744.png)
