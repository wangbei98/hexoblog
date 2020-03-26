---
title: flask-文件上传
author: bellick
copyright: true
top: 0
date: 2020-03-26 10:21:38
categories:
- flask
- flask-disk
tags:
- flask
- upload
mathjax:
---

# flask-文件上传


## 上传

### 接收文件

使用 reqparse 解析请求参数

```python
from werkzeug.datastructures import FileStorage
from flask_restful import reqparse
parse = reqparse.RequestParser()
parse.add_argument('curId',type=int,help='错误的curId',default='0')
parse.add_argument('file', type=FileStorage, location='files')

args = parse.parse_args()
# 获取当前文件夹id
cur_file_id = args.get('curId')
f = args['file']
```



## 后续工作

* 使用 Flask-Uploads 优化 https://pythonhosted.org/Flask-Uploads/
* 使用 Flask-dropzone 优化


## 参考博客
* https://www.jianshu.com/p/43bcf0ff3568
* https://stackoverflow.com/questions/51453788/flask-large-file-download
* https://flask.palletsprojects.com/en/1.0.x/patterns/streaming/
