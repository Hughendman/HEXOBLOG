---
title: flask搭建REST应用(1)
categories:
  - flask
  - python
tags:
  - flask
  - python
keywords:
  - flask
  - python
abbrlink: add72f50
date: 2018-10-17 16:15:00
---

### 安装

```
py -3 -m pip install flask

```

### 第一个服务
first.py
```
from flask import Flask
app = Flask(__name__)

@app.route('/')
def hello_world():
    return 'Hello World!'

if __name__ == '__main__':
    app.run(host='0.0.0.0', port=9033, debug=True)

```

执行： 
> python3 first.py

注：
> host='0.0.0.0'是为了让操作系统监听所有公网 IP，port为监听的端口号，debug启用调试模式注意，他们是可以分开的,在生产环境中不要使用



### 变量规则

给URL添加变量

```

@app.route('/user/<username>')
def show_user_profile(username):
    # show the user profile for that user
    return 'User %s' % username

@app.route('/post/<int:post_id>')
def show_post(post_id):
    # show the post with the given id, the id is an integer
    return 'Post %d' % post_id


```
### HTTP请求方式

默认为get方式
```
from flask import Flask,request

```
```
def do_the_login():
    return 'do_the_login'

def show_the_login_form():
    return  'show_the_login_form'

```
```
@app.route('/login', methods=['GET', 'POST'])
def login():
    if request.method == 'POST':
        return do_the_login()
    else:
        return show_the_login_form()

```

### 静态文件

```
from flask import render_template

```

```
@app.route('/hello/')
@app.route('/hello/<name>')
def hello(name=None):
    return render_template('hello.html', name=name)

```
Flask 会在 templates 文件夹里寻找模板。所以，如果你的应用是个模块，这个文件夹应该与模块同级；如果它是一个包，那么这个文件夹作为包的子目录

模板引擎采用jinja2
```
<!doctype html>
<title>Hello from Flask</title>
{% if name %}
  <h1>Hello {{ name }}!</h1>
{% else %}
  <h1>Hello World!</h1>
{% endif %}

```

### 重定向和错误

```
from flask import abort, redirect, url_for

@app.route('/')
def index():
    return redirect(url_for('login'))

@app.route('/login')
def login():
    abort(401)
    this_is_never_executed()

```

```
@app.errorhandler(404)
def page_not_found(error):
    return render_template('page_not_found.html'), 404

```