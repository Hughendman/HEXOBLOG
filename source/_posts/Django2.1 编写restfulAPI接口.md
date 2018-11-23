---
title: Django2.1编写 RESTful API 接口
categories: python
tags: python
keywords: python
abbrlink: 1c52887f
date: 2018-10-01 16:15:00
---

## 安装

```
py -3 -m pip install djangorestframework

py -3 -m pip install pygments

```

## 创建

### 创建应用

```
python3 manage.py startapp snippets

```

在setting.py下添加最后两行

```
INSTALLED_APPS = [
    'polls.apps.PollsConfig',
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

### 创建模型类

在snippets/models.py下添加

```
from django.db import models

# Create your models here.
from pygments.lexers import get_all_lexers
from pygments.styles import get_all_styles

LEXERS = [item for item in get_all_lexers() if item[1]]
LANGUAGE_CHOICES = sorted([(item[1][0], item[0]) for item in LEXERS])  # 得到所有的编程语言
STYLE_CHOICES = sorted((item, item) for item in get_all_styles())  # 得到所有的配色风格


class Snippet(models.Model):
    created = models.DateTimeField(auto_now_add=True)
    title = models.CharField(max_length=100, blank=True, default='')
    code = models.TextField()
    linenos = models.BooleanField(default=False)
    language = models.CharField(choices=LANGUAGE_CHOICES, default='python', max_length=100)
    style = models.CharField(choices=STYLE_CHOICES, default='friendly', max_length=100)

    class Meta:
        ordering = ('created',)

```

执行

```
python3 manage.py makemigrations snippets
python3 manage.py migrate

```

### 创建序列化类

使用django-rest-framework序列化库，把模型实例转化为json格式然后响应出去。

在snippets下创建serializers.py

```
from rest_framework import serializers
from snippets.models import Snippet, LANGUAGE_CHOICES, STYLE_CHOICES


class SnippetSerializer(serializers.Serializer):
    id = serializers.IntegerField(read_only=True)
    title = serializers.CharField(required=False, allow_blank=True, max_length=100)
    # 利用字段标志控制序列化器渲染到HTML页面时的的显示模板
    code = serializers.CharField(style={'base_template': 'textarea.html'})
    linenos = serializers.BooleanField(required=False)
    language = serializers.ChoiceField(choices=LANGUAGE_CHOICES, default='python')
    style = serializers.ChoiceField(choices=STYLE_CHOICES, default='friendly')

    # 给定经过验证的数据，创建并返回一个新的 Snippet 实例
    def create(self, validated_data):
        return Snippet.objects.create(**validated_data)

    # 给定经过验证的数据，更新并返回一个已经存在的 Snippet 实例
    def update(self, instance, validated_data):
        instance.title = validated_data.get('title', instance.title)
        instance.code = validated_data.get('code', instance.code)
        instance.linenos = validated_data.get('linenos', instance.linenos)
        instance.language = validated_data.get('language', instance.language)
        instance.style = validated_data.get('style', instance.style)
        instance.save()
        return instance

```

使用序列化器：

进入shell

```
python3 manage.py shell

```

```
>>> from snippets.models import Snippet
>>> from snippets.serializers import SnippetSerializer
>>> from rest_framework.renderers import JSONRenderer
>>> from rest_framework.parsers import JSONParser
>>> snippet = Snippet(code='foo = "bar"\n')
>>> snippet.save()
>>> snippet = Snippet(code='print "hello, world"\n')
>>> snippet.save()
>>> serializer = SnippetSerializer(snippet)
>>> serializer.data
>>> content = JSONRenderer().render(serializer.data)
>>> content
>>> from django.utils.six import BytesIO
>>> stream = BytesIO(content)
>>> data = JSONParser().parse(stream)
>>> serializer = SnippetSerializer(data=data)
>>> serializer.is_valid()
>>> serializer.validated_data
>>> serializer.save()
```
使用ModelSerializers

修改serializers.py：

```
class SnippetSerializer(serializers.ModelSerializer):
    class Meta:
        model = Snippet
        fields = ('id', 'title', 'code', 'linenos', 'language', 'style')

```

shell

```
>>> from snippets.serializers import SnippetSerializer
>>> serializer = SnippetSerializer()
>>> print(repr(serializer))

```

### 编写视图

snippets/views.py

```
from django.http import HttpResponse, JsonResponse
from django.views.decorators.csrf import csrf_exempt
from rest_framework.renderers import JSONRenderer
from rest_framework.parsers import JSONParser
from snippets.models import Snippet
from snippets.serializers import SnippetSerializer


# Create your views here.
@csrf_exempt
def snippet_list(request):
    """
    列出所有已经存在的snippet或者创建一个新的snippet
    """
    if request.method == 'GET':
        snippets = Snippet.objects.all()
        serializer = SnippetSerializer(snippets, many=True)
        return JsonResponse(serializer.data, safe=False)

    elif request.method == 'POST':
        data = JSONParser().parse(request)
        serializer = SnippetSerializer(data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data, status=201)
        return JsonResponse(serializer.errors, status=400)


@csrf_exempt
def snippet_detail(request, pk):
    """
    检索查看、更新或者删除一个代码段
    """
    try:
        snippet = Snippet.objects.get(pk=pk)
    except Snippet.DoesNotExist:
        return HttpResponse(status=404)

    if request.method == 'GET':
        serializer = SnippetSerializer(snippet)
        return JsonResponse(serializer.data)

    elif request.method == 'PUT':
        data = JSONParser().parse(request)
        serializer = SnippetSerializer(snippet, data=data)
        if serializer.is_valid():
            serializer.save()
            return JsonResponse(serializer.data)
        return JsonResponse(serializer.errors, status=400)

    elif request.method == 'DELETE':
        snippet.delete()
        return HttpResponse(status=204)

```

snippets/urls.py：

```
from django.conf.urls import url
from snippets import views

urlpatterns = [
    url('', views.snippet_list),
    url('(?P<pk>[0-9]+)/', views.snippet_detail),
]

```
根目录下urls.py:

```
from django.contrib import admin
from django.urls import include, path

urlpatterns = [
    path('api/snippets/', include('snippets.urls')),
    path('admin/', admin.site.urls),
]

```

### 测试

#### 安装模块

```
py -3 -m pip install httpie

```
在snippets下的admin.py添加

```
from django.contrib import admin

# Register your models here.
from .models import Snippet

admin.site.register(Snippet)

```

访问：

```
http http://127.0.0.1:8000/api/snippets/

```

返回：

```
HTTP/1.1 200 OK
Content-Length: 468
Content-Type: application/json
Date: Wed, 26 Sep 2018 03:39:16 GMT
Server: WSGIServer/0.2 CPython/3.5.4rc1
X-Frame-Options: SAMEORIGIN

[
    {
        "code": "foo = \"bar\"\n",
        "id": 1,
        "language": "python",
        "linenos": false,
        "style": "friendly",
        "title": ""
    },
    {
        "code": "print \"hello, world\"\n",
        "id": 2,
        "language": "python",
        "linenos": false,
        "style": "friendly",
        "title": ""
    },
    {
        "code": "print \"hello, world\"",
        "id": 3,
        "language": "python",
        "linenos": false,
        "style": "friendly",
        "title": ""
    }
]

```