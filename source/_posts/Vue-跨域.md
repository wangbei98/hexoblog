---
title: Vue-跨域
author: bellick
copyright: true
top: 0
date: 2020-04-12 10:43:56
categories:
- Vue
tags:
- Vue
mathjax:
---



# Vue-跨域



##  跨域



* 跨域是浏览器为了安全做出的限制措施
* 浏览器请求必须遵守同源策略：同域名，同端口，同协议



## 方法

* CORS
  * 服务端设置，前端直接调用
  * 后端允许前端某个站点进行访问
* JSONP 跨域

* 代理跨域

## Vue中的代理跨域

在vue项目的根目录下的 /config/index.js 中修改

```js
dev:{
	// 。。。
	//。。。
	// 其他配置
	
	// 设置代理
	proxyTable: {
      '/api':{
          target:'http://1x1.2xx.1xx.1xx',// ip地址
          changeOrigin:true,
          logLevel: 'debug',
          pathRewrite:{
              '/api':'/api' 
          }
      }
    }
}
```