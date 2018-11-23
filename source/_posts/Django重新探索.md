---
title: Django2.1使用Django重新探索(1)
categories: python
tags: python
keywords: python
abbrlink: c44a2080
date: 2018-09-21 16:15:00
---
## 前言

为什么是重新探索，这里给一个个人的建议，学习一个新的框架的时候，直接去看官方的文档。

## 编写你的第一个 Django 应用

### 创建项目

这里不负责讲如何安装，可以看我的上一篇文章

#### 查看版本
你可以查看版本号：我使用的是最新的
```
python3 -m django --version
```
#### 创建项目
2.1有所不同，命令如下

```
 py -3 -m django startproject mysite

```

#### 目录：

```
mysite/
    manage.py
    mysite/
        __init__.py
        settings.py
        urls.py
        wsgi.py
        
```
#### 运行

```
python3 manage.py runserver
```

##### 指定端口号

```
python3 manage.py runserver 8080
```

##### 公开IP

```
python3 manage.py runserver 0:8000

```

### 创建投票应用

注意：项目不是应用，项目可以包含很多个应用。应用可以被很多个项目使用。

#### 命令

```
python3 manage.py startapp polls
```

#### 目录

```
polls/
    __init__.py
    admin.py
    apps.py
    migrations/
        __init__.py
    models.py
    tests.py
    views.py

```

#### 编写视图

在 polls下的views.py

```
from django.http import HttpResponse


def index(request):
    return HttpResponse("Hello, world. You're at the polls index.")
    
```

在polls下创建urls.py

```

from django.urls import path

from . import views

urlpatterns = [
    path('', views.index, name='index'),
]

```

在mysite下urls.py

注意：不需要正则了

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('polls/', include('polls.urls')),
    path('admin/', admin.site.urls),
]

```

#### 测试

运行访问：http://localhost:8000/polls/

### 数据库配置

修改 mysite/settings.py

```
import pymysql
pymysql.install_as_MySQLdb()

```

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
创建默认表
```
python3 manage.py migrate

```

#### 创建模型

polls/models.py

```
from django.db import models


class Question(models.Model):
    question_text = models.CharField(max_length=200)
    pub_date = models.DateTimeField('date published')


class Choice(models.Model):
    question = models.ForeignKey(Question, on_delete=models.CASCADE)
    choice_text = models.CharField(max_length=200)
    votes = models.IntegerField(default=0)
```

#### 激活模型

mysite/settings.py
```
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
]

```

运行

```
python3 manage.py makemigrations polls
```

```
python3 manage.py sqlmigrate polls 0001
```

```
python3 manage.py migrate
```

查看数据库发现创建两个表

### API

可以打开命令行，这里不进行探索
```
python3 manage.py shell
```

修改 polls/models.py

```
import datetime

from django.db import models
from django.utils import timezone


class Question(models.Model):
    # ...
    def was_published_recently(self):
        return self.pub_date >= timezone.now() - datetime.timedelta(days=1)

```

#### 管理页面

```
python3 manage.py createsuperuser

```

启动访问：http://127.0.0.1:8000/admin/

#### 添加投票应用

在polls/admin.py

```

from django.contrib import admin

from .models import Question

admin.site.register(Question)

```

#### 添加更多视图

polls/views.py 

```
def detail(request, question_id):
    return HttpResponse("You're looking at question %s." % question_id)

def results(request, question_id):
    response = "You're looking at the results of question %s."
    return HttpResponse(response % question_id)

def vote(request, question_id):
    return HttpResponse("You're voting on question %s." % question_id)

```

polls/urls.py

```
from django.urls import path

from . import views

urlpatterns = [
    # ex: /polls/
    path('', views.index, name='index'),
    # ex: /polls/5/
    path('<int:question_id>/', views.detail, name='detail'),
    # ex: /polls/5/results/
    path('<int:question_id>/results/', views.results, name='results'),
    # ex: /polls/5/vote/
    path('<int:question_id>/vote/', views.vote, name='vote'),
]

```

再往下，我感觉没有介绍的必要了，现在都是前后端分离，如何嵌入vue项目可以看我的上一个文章。