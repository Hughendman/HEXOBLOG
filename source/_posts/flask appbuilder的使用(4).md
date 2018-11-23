---
title: flask appbuilder的使用(4)
categories:
  - flask
  - python
tags:
  - flask
  - python
keywords:
  - flask
  - python
abbrlink: 93fb4cf0
date: 2018-10-23 16:15:00
---

### 介绍
简单快速的应用开发框架，构建在Flask之上。包括详细的安全，自动CRUD生成模型，谷歌图表等等。
广泛的配置，所有功能，易于集成与Flask/Jinja2开发。
### 依赖

```
flask
click
colorama
flask-sqlalchemy
flask-login
flask-openid
flask-wtform
flask-Babel

```

### 安装

```
pip install Flask-AppBuilder

```

### 使用

#### 快速构建项目

```
fabmanager create-app
```
然后继续往下输出

#### 创建管理用户

```
fabmanager create-admin

```

#### 运行脚本

```
fabmanager run

```
或者

```
python run.py

```

#### 端口号修改
run.py

```
app.run(host='0.0.0.0', port=8080, debug=True)

```
修改port 后 执行python run.py(只适用这种方式执行)

### 基本配置

#### 数据库配置

config.py

```

SQLALCHEMY_DATABASE_URI = 'sqlite:///' + os.path.join(basedir, 'app.db')
#SQLALCHEMY_DATABASE_URI = 'mysql://myapp@localhost/myapp'
#SQLALCHEMY_DATABASE_URI = 'postgresql://root:password@localhost/myapp'


```

#### 目录结构

```
| - projectName
	| - app  
	| - babel
	| - app.db
	| - config.py
	| - README.rst
	| - run.py

```


* app目录下存放我们Web应用的相关脚本文件
* babel存放国际化相关配置和导出的翻译字符串
* 根目录下则存放着Web应用的配置脚本和主脚本文件

#### 修改名字

config.py

```
APP_NAME = "My App Name"

```
#### 主题配置
config.py

```
#APP_THEME = "bootstrap-theme.css"  # default bootstrap
#APP_THEME = "cerulean.css"
#APP_THEME = "amelia.css"
#APP_THEME = "cosmo.css"
#APP_THEME = "cyborg.css"  
#APP_THEME = "flatly.css"
#APP_THEME = "journal.css"
#APP_THEME = "readable.css"
#APP_THEME = "simplex.css"
#APP_THEME = "slate.css"   
#APP_THEME = "spacelab.css"
#APP_THEME = "united.css"
#APP_THEME = "yeti.css"


```

#### 修改首页

app/index.py

```
from flask_appbuilder import IndexView
class MyIndexView(IndexView):
    index_template = 'new_index.html'
    
```

templates/new_index.html

```
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Title</title>
</head>
<body>
    <h1>hello appbuilder</h1>
</body>
</html>

```

app/__iniy__.py指定indexview的值

```
from app.index import MyIndexView
appbuilder = AppBuilder(app, db.session, indexview=MyIndexView)

```

#### BaseView

app/views.py

```
from flask_appbuilder import AppBuilder,expose,BaseView
class indexView(BaseView):
    # 相对路径的url
    route_base = '/index'
    @expose('/hello')
    def hello(self):
        return 'Hello World'
appbuilder.add_view_no_menu(indexView())

```
添加权限@has_access必须登录

```
from flask_appbuilder import ModelView,AppBuilder,expose,BaseView,has_access
class indexView(BaseView):
    route_base = '/index'
    @expose('/hello')
    def hello(self):
        return 'Hello World'
    @expose('/message/<string:msg>')
    @has_access
    def message(self,msg):
        msg = 'Hello' + msg
        return msg
appbuilder.add_view_no_menu(indexView())

```

#### 添加视图到菜单
app/templates/index.html
```
{% extends "appbuilder/base.html" %}
{% block content %}
    <h1>Welcome</h1>
    <h2>Hello World</h2>
    <h3>{{ msg }}</h3>
{% endblock %}

```
app/views.py

```
class indexView(BaseView):
    route_base = '/index'
    default_view = 'hello'
    @expose('/hello')
    def hello(self):
        return 'Hello World'
    @expose('/message/<string:msg>')
    @has_access
    def message(self,msg):
        msg = 'Hello ' + msg
        return self.render_template('index.html',msg=msg)
# 在菜单中生成访问的链接
appbuilder.add_view(indexView,'Hello',category='My View')
appbuilder.add_link('message',href='/index/message/user',category='My View')
appbuilder.add_link('welcome',href='/index/hello',category='My View')

```

#### ModelView
app/models.py

```
from sqlalchemy import Column, Integer, String, ForeignKey, Date
from sqlalchemy.orm import relationship
from flask_appbuilder import Model
 

class ContactGroup(Model):
    id = Column(Integer, primary_key=True)
    name = Column(String(50), unique=True, nullable=False)

    def __repr__(self):
        return self.name


class Contact(Model):
    id = Column(Integer, primary_key=True)
    name = Column(String(150), unique=True, nullable=False)
    address = Column(String(564))
    birthday = Column(Date)
    personal_phone = Column(String(20))
    personal_cellphone = Column(String(20))
    contact_group_id = Column(Integer, ForeignKey('contact_group.id'))
    contact_group = relationship("ContactGroup")

    def __repr__(self):
        return self.name



```

app/views.py 

```
from flask_appbuilder import ModelView
from flask_appbuilder.models.sqla.interface import SQLAInterface
from .models import ContactGroup, Contact
from app import appbuilder, db
 
class ContactModelView(ModelView):
    datamodel = SQLAInterface(Contact)
 
    label_columns = {'contact_group':'Contacts Group'}
    list_columns = ['name','personal_cellphone','birthday','contact_group']
 
    show_fieldsets = [
                        (
                            'Summary',
                            {'fields':['name','address','contact_group']}
                        ),
                        (
                            'Personal Info',
                            {'fields':['birthday','personal_phone','personal_cellphone'],'expanded':False}
                        ),
                     ]
# 在联系人组视图中，我们使用related_views来关联联系人视图，F.A.B.将自动处理他们之间的关系。
class GroupModelView(ModelView):
    datamodel = SQLAInterface(ContactGroup)
    related_views = [ContactModelView]
 
# 现在我们就差最后一步工作就要完成本次实验了。
# 首先使用db.create_all()根据数据库模型创建表，然后再将视图添加到菜单。
 
db.create_all()
appbuilder.add_view(GroupModelView,
                    "List Groups",
                    icon = "fa-address-book-o",
                    category = "Contacts",
                    category_icon = "fa-envelope")
appbuilder.add_view(ContactModelView,
                    "List Contacts",
                    icon = "fa-address-card-o",
                    category = "Contacts")


```

## 参考

[Flask-AppBuilder教程](https://mathpretty.com/9304.html)

[利用Flask-AppBuilder 快速构建Web后台管理应用](https://blog.csdn.net/oxuzhenyi/article/details/77586500)