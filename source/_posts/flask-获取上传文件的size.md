---
title: flask-获取上传文件的size
author: bellick
copyright: true
top: 0
date: 2020-03-29 16:14:03
categories:
- flask
- flask-disk
tags:
- flask
- 上传文件
mathjax:
---

# flask-获取上传文件的size


```python

parse = reqparse.RequestParser()
parse.add_argument('file', type=FileStorage, location='files')

args = parse.parse_args()

f = args['file']

fsize = len(f.read())

```

很不幸，上述方法不可行， 如此获取文件大小，会导致如下问题:
在打开此文件时，会报错
下载文件也会报错

```
PIL.UnidentifiedImageError: cannot identify image file '/root/cloud-disk/backend/upload/26a263699915151f56c0fbbe6e4ac473'
```


因此直接用下面的方法:
反正是要保存的，就在保存之后再获取文件大小吧

```python
f.save(target_file)

fsize = os.path.getsize(target_file)
```