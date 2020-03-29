---
title: flask-用token认证
author: bellick
copyright: true
top: 0
date: 2020-03-29 11:22:21
categories:
- flask
- flask-disk
tags:
- flask
- token
- 认证
mathjax:
---

# flask-用token认证

## 问题背景

在网盘项目中，需要给用提供预览图片的功能。一个必须的条件是：用户只能访问自己的上传的图片。因此，在编写预览API的时候，必须实现用户认证，在其他API中，我使用的是 flask-login 包中的 login-required 装饰器来保证用户已经登录。但此 flask-login 是通过 cookies 和session来存储用户登录信息，从而实现用户认证。但在此问题中，客户端实现图片预览的api是  Image.network(url)，加载过程是不可见的。所以没办法在此请求中挂载 cookies ，因而 login-required 不能使用。

## 实现思路

现在的思路是通过在url 中添加查询字符串，在url中携带token来实现用户认证，这一点是参考 cowtransfer(奶牛快传) 的文件分享的 url格式。

在用户登录的时候，给前端返回一个属于他的token放在cookie里，前端在预览的时候把这个token带过去，在预览API中，解析URL中的token来获得发送请求的用户信息。这样就可以实现用户认证了


## 代码

```python
# utils.py
from itsdangerous import TimedJSONWebSignatureSerializer as Serializer
from flask_restful import reqparse
from flask import jsonify
from settings import config
from models import UserTable

import functools


# 根据用户id生成token
def generate_token(uid):
	s = Serializer(config['SECRET_KEY'],config['EXPIRES_TIME'])
	token = s.dumps({"uid":uid}).decode("ascii")
	return token

# 验证token，如果验证通过，则返回token代表的用户，如果不通过，则返回None
def verify_token(token):
	s = Serializer(config['SECRET_KEY'])
	try:
		data = s.loads(token)
	except Exception:
		return None
	try:
		user = UserTable.query.get(data['uid'])
	except:
		return None
	if user:
		return user
	else:
		return None

# 写一个装饰器
def token_required(view_func):
	@functools.wraps(view_func)
	def verify_token(*args,**kwargs):
		parse = reqparse.RequestParser()
		parse.add_argument('token',type=str,help='wrong token')
		args = parse.parse_args()

		token = args.get('token')
		
		s = Serializer(config['SECRET_KEY'])
		try:
			s.loads(token)
		except Exception:
			return jsonify(code = 38,msg = "wrong token")

		return view_func(*args,**kwargs)

	return verify_token


```

```python
#settings.py

config={
	'SECRET_KEY':'xxxxxx',
	'EXPIRES_TIME':86400,
}
```

## 使用

```python

class Login(Resource):
	def get(self):
		xxxxxx
		token = generate_token(current_user.uid)
		xxxxxx
		response.set_cookie('token',token)
		return response
class PreviewAPI(Resource):
	@token_required
	def get(self):
		parse = reqparse.RequestParser()
		parse.add_argument('token',type=str,help='wrong token')
		args = parse.parse_args()

		token = args.get('token')
		try:
			# 解析token中的用户信息
			user = verify_token(token)


```