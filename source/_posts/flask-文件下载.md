---
title: flask-文件下载
author: bellick
copyright: true
top: 0
date: 2020-03-26 10:30:32
categories:
- flask
- flask-disk
tags:
- flask
mathjax:
---

# flask-文件下载

## 下载
使用 send_file 函数
```python
from flask import send_file

if os.path.exists(target_file):
	return send_file(target_file,as_attachment=True,attachment_filename=filename,cache_timeout=3600)
```


## 其他想法

如果文件过大，则使用流处理

```python
def generate(self,path):
	with open(path, 'rb') as fd:
		while 1:
			buf = fd.read(CHUNK_SIZE)
			if buf:
				yield buf
			else:
				break

response =  Response(stream_with_context(self.generate(target_file)),content_type='application/octet-stream')
response.headers["Content-disposition"] = "attachment; filename={}".format(filename.encode().decode('latin-1'))
return response
```



## 参考博客

* https://blog.csdn.net/lucyxu107/article/details/96480902
* https://blog.csdn.net/hunyxv/article/details/103031547