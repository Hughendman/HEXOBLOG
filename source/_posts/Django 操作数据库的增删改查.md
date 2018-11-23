---
title: Django 操作数据库的增删改查
categories: python
tags: python
keywords: python
abbrlink: 9ed051d
date: 2018-10-05 16:15:00
---

### 创建应用

#### 创建一个应用vacation

```
python3 manage.py startapp vacation

```
#### 在setting.py下添加
    'vacation.apps.VacationConfig'
```


INSTALLED_APPS = [
    'polls.apps.PollsConfig',
    'vacation.apps.VacationConfig',
    'person.apps.PersonConfig',
    'django.contrib.admin',
    'django.contrib.auth',
    'django.contrib.contenttypes',
    'django.contrib.sessions',
    'django.contrib.messages',
    'django.contrib.staticfiles',
    'rest_framework',
    'snippets.apps.SnippetsConfig',
]

```

#### 在vacation下的model.py下添加

```
from django.db import models

# Create your models here.


class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    area = models.TextField()
    count = models.BooleanField(default=False)

    class Meta:
        ordering = ('created',)

```

执行：

```
python3 manage.py makemigrations vacation
python3 manage.py migrate

```

#### 创建序列化类
在vacation下创建serializers.py


```
from rest_framework import serializers
from vacation.models import Snippet


class VacationSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    # 利用字段标志控制序列化器渲染到HTML页面时的的显示模板
    area = serializers.CharField(style={'base_template': 'textarea.html'})
    count = serializers.BooleanField(required=False)

    # 给定经过验证的数据，创建并返回一个新的 Snippet 实例
    def create(self, validated_data):
        return Snippet.objects.create(**validated_data)

    # 给定经过验证的数据，更新并返回一个已经存在的 Snippet 实例
    def update(self, instance, validated_data):
        instance.title = validated_data.get('title', instance.title)
        instance.area = validated_data.get('area', instance.area)
        instance.count = validated_data.get('count', instance.count)
        instance.save()
        return instance

```
#### 编写视图
vacation/views.py
```
from django.shortcuts import render

# Create your views here.
from django.http import HttpResponse, JsonResponse
from django.views.decorators.csrf import csrf_exempt
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
from vacation.models import Snippet
from vacation.serializers import VacationSerializer


# Create your views here.
@csrf_exempt
def vacation_list(request):
    """
    列出所有已经存在的vacation
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = VacationSerializer(snippets, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = VacationSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data, status=201)
        return JsonResponse(serializer.errors, status=400)


```


vacation/urls.py：

```
from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('', views.vacation_list),
]

```

根目录/urls.py

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('api/vacation/', include('vacation.urls')),
    path('admin/', admin.site.urls),
]

```

访问：http://127.0.0.1:8000/api/vacation/

在vacation下的admin.py添加

```
from django.contrib import admin

# Register your models here.
from .models import Snippet

admin.site.register(Snippet)

```

访问:http://127.0.0.1:8000/admin

### 数据库操作--查询

#### 查询所有

view.py

```
@csrf_exempt
def vacation_list(request):
    """
    列出所有已经存在的vacation
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = VacationSerializer(snippets, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = VacationSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data, status=201)
        return JsonResponse(serializer.errors, status=400)




```

urls.py

```
from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('list/', views.vacation_list)
]

```

访问：http://127.0.0.1:8000/api/vacation/list/


#### 根据id查询

view.py

```

@csrf_exempt
def vacation_id(request):
    resId = request.GET.get('id', '')
    snippets = Snippet.objects.filter(id=resId)
    serializer = VacationSerializer(snippets, many=True)
    return JsonResponse(serializer.data, safe=False)


```

urls.py

```
from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('list/', views.vacation_list),
    url('id/', views.vacation_id),
]

```

访问：http://127.0.0.1:8000/api/vacation/id/?id=1

### 更新数据

#### 根据id更新数据

view.py

```
@csrf_exempt
def vacation_update(request):
    try:
        resId = request.GET.get('id', '')
        resTitle = request.GET.get('title', '')
        Snippet.objects.filter(id=resId).update(title=resTitle)
        return JsonResponse({"status": "success"}, safe=False)
    except Exception as err:
        print(err)
        return JsonResponse({"status": "false"}, safe=400)

```

urls.py

```
from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('list/', views.vacation_list),
    url('id/', views.vacation_id),
    url('update/', views.vacation_update),
]

```

访问：http://127.0.0.1:8000/api/vacation/update?id=1&title=端午

### 增加数据

view.py

```
@csrf_exempt
def vacation_insert(request):
    try:
        resTitle = request.GET.get('title', '')
        resArea = request.GET.get('area', '')
        resCount = request.GET.get('count', '')
        Snippet.objects.create(title=resTitle,area=resArea,count=resCount)
        return JsonResponse({"status": "success"}, safe=False)
    except Exception as err:
        print(err)
        return JsonResponse({"status": "false"}, safe=400)

```
urls.py

```
from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('list/', views.vacation_list),
    url('id/', views.vacation_id),
    url('update/', views.vacation_update),
    url('insert/', views.vacation_insert),
]

```

访问：http://127.0.0.1:8000/api/vacation/insert/?title=端午&area=北京&count=True

### 删除数据

view.py

```
@csrf_exempt
def vacation_delete(request):
    try:
        resId = request.GET.get('id', '')
        Snippet.objects.filter(id=resId).delete()
        return JsonResponse({"status": "success"}, safe=False)
    except Exception as err:
        print(err)
        return JsonResponse({"status": "false"}, safe=400)

```

urls.py

```

from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('list/', views.vacation_list),
    url('id/', views.vacation_id),
    url('update/', views.vacation_update),
    url('insert/', views.vacation_insert),
    url('delete/', views.vacation_delete)
]


```

访问：http://127.0.0.1:8000/api/vacation/delete/?id=1

不用操作sql直接就可以操作数据库确实很方便，但是我就想写sql，怎么办？

### 直接写sql

```

@csrf_exempt
def my_custom_sql(request):
    try:
        from django.db import connection, transaction
        cursor = connection.cursor()
        cursor.execute("SELECT * FROM	vacation_snippet")
        row = cursor.fetchone()
        return JsonResponse(row, safe=False)
    except Exception as err:
        print(err)
        return JsonResponse({"status": "false","msg":err}, safe=400)

```

```
from django.conf.urls import url
from vacation import views

urlpatterns = [
    url('list/', views.vacation_list),
    url('id/', views.vacation_id),
    url('update/', views.vacation_update),
    url('insert/', views.vacation_insert),
    url('delete/', views.vacation_delete),
    url('sql/', views.my_custom_sql)
]

```

访问：http://127.0.0.1:8000/api/vacation/sql