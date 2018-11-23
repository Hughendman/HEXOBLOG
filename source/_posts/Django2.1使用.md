---
title: Django2.1使用
categories: python
tags: python
keywords: python
abbrlink: 218a770a
date: 2018-09-20 16:15:00
---

## 介绍

Python下有许多款不同的 Web 框架。Django是重量级选手中最有代表性的一位。许多成功的网站和APP都基于Django。

Django是一个开放源代码的Web应用框架，由Python写成。（来自菜鸟）

## 安装

```
py -3 -m pip install Django

```

## 创建项目

```
django-admin.py startproject HelloWorld

```
然后你发现并不好使，找了半天，发现是执行下面这句

```
 py -3 -m django startproject yinxs

```

接着执行

```
 python3 .\manage.py startup webdev

```

发现又不好使了

执行：

```
 python3 .\manage.py startapp webdev

```

启动项目

```

python3 .\manage.py runserver 0.0.0.0:8000

```
之所以是0.0.0.0是为了让其他计算机也可以访问到

访问：http://127.0.0.1:8000/

这句命令并没有改变

## 目录介绍

如果是按照上面的步骤，那么咱们的目录应该是一样的，就算有一些修改，但是大体上没有影响

```
__init__.py：让Python把该目录当成一个标准的开发包；

settings.py：django项目的配置文件；

urls.py：django项目的URL配置文件(尝试访问http://127.0.0.1:8000/admin/login/?next=/admin/)；

wsgi.py：wsgi是Python语言定义的web服务器，为项目提供的一种服务接口；

manage.py：命令行工具，可以用多种方式与该django项目进行交互；

webdev: 新建的应用
```

我们进入webdev文件

```
migrations/：记录models中的数据变更；

admin.py：映射models中的数据到admin后台；

apps.py：对创建的应用进行配置，比如新增文件；

models.py：Django模型文件，创建应用程序的数据表模型；

tests.py：创建测试用例；

views.py：Django视图文件，控制向前端页面传输的内容；

```

## vue + django

前端的话还是老样子，使用vue，下面我来讲解如何将Django2.1和vue结合到一起

### 构建vue项目

```
cnpm install -g vue-cli

```

```
vue-init webpack fronted

```

```
cd fronted

```

```
cnpm i

```

```
npm run build

```

### 准备工作-解决跨域问题

```
py -3 -m pip install django-cors-headers

```

修改settings.py

```
MIDDLEWARE = [
    'django.middleware.security.SecurityMiddleware',
    'django.contrib.sessions.middleware.SessionMiddleware',
    'corsheaders.middleware.CorsMiddleware',
    'django.middleware.common.CommonMiddleware',
    'django.middleware.csrf.CsrfViewMiddleware',
    'django.contrib.auth.middleware.AuthenticationMiddleware',
    'django.contrib.messages.middleware.MessageMiddleware',
    'django.middleware.clickjacking.XFrameOptionsMiddleware',
]

CORS_ORIGIN_ALLOW_ALL = True

```

### 耦合

修改urls.py

Django新版本改变导致URL中不需要再使用正则表达式了，只需要路径就OK了。

```


urlpatterns = [
    path('admin/', admin.site.urls),
    path('', TemplateView.as_view(template_name="index.html")),
]

```

修改settings.py


```
TEMPLATES = [
    {
        'BACKEND': 'django.template.backends.django.DjangoTemplates',
        'DIRS': ['fronted/dist'],
        'APP_DIRS': True,
        'OPTIONS': {
            'context_processors': [
                'django.template.context_processors.debug',
                'django.template.context_processors.request',
                'django.contrib.auth.context_processors.auth',
                'django.contrib.messages.context_processors.messages',
            ],
        },
    },
]

```

静态文件的搜索路径:

```
STATICFILES_DIRS = [
    os.path.join(BASE_DIR, "fronted/dist/static"),
]

```

启动项目：

```
python3 .\manage.py runserver 0.0.0.0:9000

```

访问：http://127.0.0.1:9000

## 数据库
 
### 安装

前后端融合完成，下面我们来写几个接口，首先我们要配置数据库。

```
py -3 -m pip install mysqlclient

```
失败：

[whl下载地址](https://www.lfd.uci.edu/~gohlke/pythonlibs/#mysqlclient)

下载文件，执行

```
py -3 -m pip install C:\Users\admin\Downloads\mysqlclient-1.3.13-cp
35-cp35m-win_amd64.whl

```
注意你的版本号

```
py -3 -m pip install PyMySQL

```
修改Django的工程同名子目录的init.py文件

```
from pymysql import install_as_MySQLdb
install_as_MySQLdb()

```

修改settings.py

```
DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'myproject',
        'USER': 'root',
        'PASSWORD': 'root',
        'HOST': '127.0.0.1',
        'PORT': '3306'
    }
}

```

### 操作

```
python3 manage.py shell

```

## 快速创建应用

```
python3 manage.py startapp polls

```

在polls下的views.py

```
from django.http import HttpResponse
 
 
def index(request):
    return HttpResponse("Hello world!")


```

在polls下创建urls.py

```
from django.urls import path
 
from . import views
 
urlpatterns = [
    path('', views.index, name='index'),
]


```

在根目录下的urls.py中

```
from django.contrib import admin
from django.urls import path,include
from django.views.generic.base import TemplateView

urlpatterns = [
	path('admin/', admin.site.urls),
    path('', TemplateView.as_view(template_name="index.html")),
    path('polls/', include('polls.urls')),
]


```

访问： http://127.0.0.1:8000/admin/login/?next=/admin/

当然访问这个要先创建一个管理员账号

## 管理员账号

```
python manage.py createsuperuser

```