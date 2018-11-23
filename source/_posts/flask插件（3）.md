---
title: flask插件（3）
categories:
  - flask
  - python
tags:
  - flask
  - python
keywords:
  - flask
  - python
abbrlink: d70bcc7a
date: 2018-10-18 16:15:00
---

### Flask-appbuilder

#### 介绍

简单快速的应用开发框架，构建在Flask之上。包括详细的安全，自动CRUD生成您的模型。

#### 安装

```
pip install flask-appbuilder
```

### Flask-login

#### 介绍
Flask- login为Flask提供用户会话管理。它处理登录、注销和在较长时间内记住用户会话的常见任务。

#### 安装

```
pip install flask-login
```

### Flask-sqlalchemy

#### 介绍

向flask应用程序添加SQLAlchemy支持。

#### 安装
```
pip install Flask-SQLAlchemy

```

### flask-openid

#### 介绍

Adds OpenID support to Flask.

#### 安装
```
pip install Flask-OpenID

```

### flask-babel

#### 介绍
Adds i18n/l10n support to Flask applications with the help of the Babel library.

#### 安装

```
pip install Flask-Babel

```

### Flask-WTF

#### 介绍

Flask和WTForms的简单集成，包括CSRF、文件上传和reCAPTCHA。

#### 安装

```
pip install Flask-WTF
```
### flask-Script

#### 介绍

终端运行解析器

#### 安装

```
pip install flask-script
```

#### 使用 

```
from flask_script import Manager
from flask import Flask

app = Flask(__name__)
manager = Manager(app)

# 启动实例
if __name__ == '__main__':
    manager.run()


```
运行

```
python manage.py runserver
python manage.py runserver -d -r --thread

```
启动参数
```
  -?,--help # 查看启动设置帮助
   -h,--host # 指定主机
   -p,--port # 指定端口
   --thread # 启动多线程
   -d # 开启调试模式
   -r # 代码修改后自动加载
```

### flask-bootstrap

#### 介绍

bootstrap 是twitter开发的一个开源框架，在程序中集成bootstrap就要用的这个插件flask-bootstrap


#### 安装

```
pip install flask_bootstrap

```
#### 使用

```
from flask.ext.bootstrap import Bootstrap
bootstrap = Bootstrap(app)

```

```
{%extends "bootstrap/base.html"%}

{%block title %}Flask{% endblock %}

{%block navbar %}
<div class="navbar navbar-inverse" role="navigation">
    <div class="container">
        <div class="navbar-header">
            <button type="button" class="navbar-toggle"
            data-toggle="collapse" data-target=".navbar-collapse">
                <span class="sr-only">Toggle navigation</span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
            <a class="navbar-brand" href="/">Flasky</a>
        </div>
        <div class="navbar-collapse collapse">
            <ul class="nav navbar-nav">
                <li><a href="/">Home</a></li>
            </ul>
        </div>
    </div>
</div>

{% endblock %}
{% block content %}
<div class="container">
    <div class="page-header">
        <h1>Hello, {{ name }}!</h1>
    </div>
</div>
{% endblock %}

```

### Flask-Mail

#### 介绍

#### 安装
```
pip install flask-mail

```

### Flask-Moment

#### 介绍

本地化时间 

#### 安装

```

pip install flask-moment 

```

#### 使用

```
# 导入类库 
from flask_moment import Moment 
from datetime import datetime, timedelta 

# 创建对象 
moment = Moment(app) 

@app.route('/moment/') 

def mom(): 
    current_time = datetime.utcnow() + timedelta(seconds=-3600) 
    return render_template('moment.html', current_time=current_time)
    
```

```
{# 简单的格式化显示 #}
   <div>时间：{{ moment(current_time).format('LLLL') }}</div>
   <div>时间：{{ moment(current_time).format('LLL') }}</div>
   <div>时间：{{ moment(current_time).format('LL') }}</div>
   <div>时间：{{ moment(current_time).format('L') }}</div>

   {# 自定义格式化显示 #}
   <div>自定义显示：{{ moment(current_time).format('YYYY-MM-DD') }}</div>

   {# 时间差值显示 #}
   <div>发表于：{{ moment(current_time).fromNow() }}</div>

   {# 加载jQuery，因为moment.js依赖，使用bootstrap时可以省略 #}
   {{ moment.include_jquery() }}

   {# 加载moment.js #}
   {{ moment.include_moment() }}

   {# 设置中文显示 #}
   {{ moment.locale('zh-CN') }}

```

### Flask-Uploads

#### 介绍

上传文件

#### 安装

```
pip install flask-uploads

```


### 自动生成依赖包

```
pip freeze > requirements.txt

```

### 安装依赖包

```
pip install -r Requirements.txt

```

## 没有使用方法的插件将在其他文档中说明

# 参考

[Flask常用扩展](https://blog.csdn.net/lm_is_dc/article/details/80859190?utm_source=blogxgwz0)