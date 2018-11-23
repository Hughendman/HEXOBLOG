---
title: Django2.1使用Django重新探索(模型层)
categories: python
tags: python
keywords: python
abbrlink: 3f0e5b98
date: 2018-09-22 16:15:00
---

## 模型

### 介绍

模型是您的数据唯一而且准确的信息来源。它包含您正在储存的数据的重要字段和行为。一般来说，每一个模型都映射一个数据库表。

* 每个模型都是一个 Python 的类，这些类继承 django.db.models.Model
* 模型类的每个属性都相当于一个数据库的字段。

### 创建应用

```
python3 manage.py startapp person

```

在 person/models.py

```
from django.db import models

class Person(models.Model):
    first_name = models.CharField(max_length=30)
    last_name = models.CharField(max_length=30)

```
mysite/settings.py
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
]


```

执行： 

```
python3 manage.py makemigrations person
python3 manage.py sqlmigrate person 0001
python3 manage.py migrate
```

检查数据库，发现多了一个person_person表

### 字段

模型中最重要的、并且也是唯一必须的是数据库的字段定义。字段在类中定义。定义字段名时应小心避免使用与 models API</ref/models/instances>冲突的名称， 如 ``clean`, save, or ``delete``等.

ex:

```
from django.db import models

class Musician(models.Model):
    first_name = models.CharField(max_length=50)
    last_name = models.CharField(max_length=50)
    instrument = models.CharField(max_length=100)

class Album(models.Model):
    artist = models.ForeignKey(Musician, on_delete=models.CASCADE)
    name = models.CharField(max_length=100)
    release_date = models.DateField()
    num_stars = models.IntegerField()

```