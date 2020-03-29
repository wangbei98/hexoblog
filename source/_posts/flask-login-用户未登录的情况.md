---
title: flask-login-用户未登录的情况
author: bellick
copyright: true
top: 0
date: 2020-03-28 22:29:49
categories:
- flask
- flask-login
tags:
mathjax:
---

# flask-login-用户未登录的情况

用 LoginManager.unauthorized_handler 装饰 一个函数:

```python
@login_manager.unauthorized_handler
def unauthorized():
    # do stuff
    return a_response
```