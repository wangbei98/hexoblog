---
title: flask-预览图片
author: bellick
copyright: true
top: 0
date: 2020-03-26 00:33:34
categories:
- flask
- flask-disk
tags:
- flask
mathjax:
---

## flask-disk 图片预览

* 使用 Pillow 处理图片
```python
from PIL import Image

img_data = Image.open(target_file)
img_data.thumbnail((width,height))
```
* Image对象转换为比特流
```python
import io

fp = io.BytesIO()
format = Image.registered_extensions()['.'+node_type]
img_data.save(fp, format)

response = make_response(fp.getvalue())
```

```python
class PreviewAPI(Resource):
	def get(self):
		parse = reqparse.RequestParser()
		parse.add_argument('id',type=int,help='错误的id',default='0')
		parse.add_argument('width',type=int,help='wrong width',default=300)
		parse.add_argument('height',type=int,help='wrong height',default=300)
		args = parse.parse_args()
		# 获取当前文件夹id
		file_id = args.get('id')
		width = args.get('width')
		height = args.get('height')
		try:
			file_node = FileNode.query.get(file_id)
		except:
			return jsonify(code = 11,message='node not exist, query fail')
		if file_node == None:
			return jsonify(code = 11,message='node not exist, query fail')
		if file_node.type_of_node in ['jpeg','jpg','png']:
			parent_id = file_node.parent_id
			filename = file_node.filename
			node_type = file_node.type_of_node
				# 生成文件名的 hash
			actual_filename = generate_file_name(parent_id, filename)
				# 结合 UPLOAD_FOLDER 得到最终文件的存储路径
			target_file = os.path.join(os.path.expanduser(UPLOAD_FOLDER), actual_filename)
			if os.path.exists(target_file):
				img_data = Image.open(target_file)
				img_data.thumbnail((width,height))

				fp = io.BytesIO()
				format = Image.registered_extensions()['.'+node_type]
				img_data.save(fp, format)

				response = make_response(fp.getvalue())
				response.headers['Content-Type'] = 'image/' + node_type
				return response
			else:
				return jsonify(code='22',message='file not exist')
		else:
			return jsonify(code = 24,message='preview not allowed')
```
