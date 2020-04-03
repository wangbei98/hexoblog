---
title: flask-login-权限判定
author: bellick
copyright: true
top: 0
date: 2020-04-03 13:32:09
categories:
- flask
- flask-login
tags:
mathjax:
---

# flask-login-权限判定

## 问题背景

在进行用户认证的时候，flask-login 是个很方便的轮子，但我现在不仅需要认证，还需要对用户的身份进行鉴别，区分不同用户的权限。
比如有些操作不需要用户登录，有写操作所有登录用户都可以，而有些操作，只有拥有特定权限的用户才能执行。

## 准备工作

user表要继承 UserMixin,is_admin 表示用户是不是管理员，1 表示「是」,0 表示「不是」
现在需要的是，有些操纵只能管理员执行，也就是 is_admin = 1 的用户

```python
class UserTable(UserMixin, db.Model):
    __tablename__ = 'user'
    uid = db.Column(db.Integer, primary_key=True)
    password_hash = db.Column(db.String(200), nullable=False)
    username = db.Column(db.String(20))
    is_admin = db.Column(db.Integer, default=0)

```

## 模仿 login-required 写一个装饰器

* flask-login 装饰器（源码）

```python
def login_required(func):
    '''
    If you decorate a view with this, it will ensure that the current user is
    logged in and authenticated before calling the actual view. (If they are
    not, it calls the :attr:`LoginManager.unauthorized` callback.) For
    example::
        @app.route('/post')
        @login_required
        def post():
            pass
    If there are only certain times you need to require that your user is
    logged in, you can do so with::
        if not current_user.is_authenticated:
            return current_app.login_manager.unauthorized()
    ...which is essentially the code that this function adds to your views.
    It can be convenient to globally turn off authentication when unit testing.
    To enable this, if the application configuration variable `LOGIN_DISABLED`
    is set to `True`, this decorator will be ignored.
    .. Note ::
        Per `W3 guidelines for CORS preflight requests
        <http://www.w3.org/TR/cors/#cross-origin-request-with-preflight-0>`_,
        HTTP ``OPTIONS`` requests are exempt from login checks.
    :param func: The view function to decorate.
    :type func: function
    '''
    @wraps(func)
    def decorated_view(*args, **kwargs):
        if request.method in EXEMPT_METHODS:
            return func(*args, **kwargs)
        elif current_app.config.get('LOGIN_DISABLED'):
            return func(*args, **kwargs)
        elif not current_user.is_authenticated:
            return current_app.login_manager.unauthorized()
        return func(*args, **kwargs)
    return decorated_view
```

* admin_required 装饰器

```python

from flask_login import current_user
from flask import make_response,jsonify
from functools import wraps

def admin_required(func):
	@wraps(func)
	def decorated_view(*args, **kwargs):
		if not current_user.is_authenticated:
			# print(current_user.authority)
			return current_app.login_manager.unauthorized()

		if current_user.is_admin == 0:
			return jsonify(code=36,message='permission denied')
		return func(*args, **kwargs)
	return decorated_view

```

## 使用

* 所有用户都可以的操作不需要加装饰器
* 所有登录用户都可以的操作加 @login_required
* 管理员用户才能执行的操作加 @admin_required

## 参考博客

[装饰器](https://www.jianshu.com/p/ee82b941772a)
[权限判定](https://www.jianshu.com/p/2341064111a8)

[实现token](https://www.jb51.net/article/173722.htm)

[flask-login源码](https://github.com/maxcountryman/flask-login/blob/master/flask_login/utils.py)