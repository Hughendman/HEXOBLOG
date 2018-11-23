---
title: Superset搭建及其简单使用
categories: Superset
tags: Superset
keywords: Superset
abbrlink: '24650887'
date: 2018-10-10 16:15:00
---
## 安装

```
#安装Superset 
pip install superset 

export PYTHONPATH=/usr/local/lib/python3.5/site-packages:$PYTHONPATH

 
#创建管理员用户名和密码 
fabmanager create-admin --app superset 
 
#初始化Superset 

superset db upgrade 
 
#装载初始化样例数据 
superset load_examples 
 
#创建默认角色和权限 
superset init 
 
#启动Superset 
superset runserver 

(window)
C:\Users\admin\AppData\Local\Programs\Python\Python35\Lib\site-packages\superset-0.27.0-py3.5.egg\superset\bin

python superset runserver -d 

```

## 汉化

```
superset-0.17.1-py3.5.egg/superset/config.py

```

```
# ---------------------------------------------------
# Babel config for translations
# ---------------------------------------------------
# Setup default language
BABEL_DEFAULT_LOCALE = 'zh'
# Your application default translation path
BABEL_DEFAULT_FOLDER = 'superset/translations'
# The allowed translation for you app
LANGUAGES = {
    'en': {'flag': 'us', 'name': 'English'},
    'it': {'flag': 'it', 'name': 'Italian'},
    'fr': {'flag': 'fr', 'name': 'French'},
    'zh': {'flag': 'cn', 'name': 'Chinese'},
    'ja': {'flag': 'jp', 'name': 'Japanese'},
    'de': {'flag': 'de', 'name': 'German'},
    'pt_BR': {'flag': 'br', 'name': 'Brazilian Portuguese'},
    'ru': {'flag': 'ru', 'name': 'Russian'},
}

```

## 使用

### 配置数据源

Sources >> Databases

在Database中添加名字

在SQLALCHEMY URI中添加下面格式
```
mysql+mysqldb://用户名:密码@IP:端口号/数据库名称?charset=utf8

```
选择在sql工具箱公开这个数据库

### SQL Lab

SQL Lab >> SQL Editor

选择规格的数据库，可以编写sql了

查询后点击可视化

可视化配置完成后，保存

## 登录问题

步骤：

1.通过iframe引用superset的图表。


2.找到superset项目内的 config.py文件，
PUBLIC_ROLE_LIKE_GAMMA = False， 把它设置为True

3.进入superset，
导航内找到 security ，点击list roles，看到public，点击edit。

4.最后在permissions里把以下三个加上

```
can explore on Superset
can explore json on Superset
all database access on all_database_access

```

5.保存，退出登陆。


