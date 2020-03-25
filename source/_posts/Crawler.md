---
title: Crawler
author: bellick
copyright: true
top: 0
date: 2019-04-29 15:17:56
categories:
- Notebook
- Python
tags:
mathjax:
---
request
```
 import requests
 newsurl = ‘https://news.sina.com.cn/’
 response = request.get(newsurl)
 type(response)
 # requests.models.Response
 type(response.text)
 # str
```
urllib 
```
 import urllib
 response = urllib.response.urlopen(’http:php.net’)
 response.encoding=‘utf-8’ # 改变编码方式
 type(response)
 # http.client.HTTPResponse
 html = response.read()
 type(html)
 # bytes
 ```