---
title: Django2.1使用Django重新探索(工具)
categories: python
tags: python
keywords: python
abbrlink: 2a237a1c
date: 2018-09-30 16:15:00
---
## admin 中文设置

在setttings.py下

```
# LANGUAGE_CODE = 'en-us'

LANGUAGE_CODE = 'zh-Hans'
# TIME_ZONE = 'UTC'
TIME_ZONE = 'CCT'

USE_I18N = True

USE_L10N = True

USE_TZ = True
```
访问：http://127.0.0.1:8000/admin/
发现已经变成中文的了

## 认证

### 创建普通用户
```
python3 manage.py shell

```
```
>>> from django.contrib.auth.models import User
>>> user = User.objects.create_user('yinxss', 'yinxs@126.com', '12345678')


```

### 创建超级用户

```
$ python manage.py createsuperuser --username=hugh --email=hugh@126.com

```

### 更改密码

```
>>> from django.contrib.auth.models import User
>>> u = User.objects.get(username='yinxss')
>>> u.set_password('zxlkdfjeirr')
>>> u.save()


```

### 验证用户

```
>>> from django.contrib.auth import authenticate
>>> user = authenticate(username='yinxss', password='zxlkdfjeirr')
>>> print(user)

```

### 权限和认证

admin管理员的权限：添加、修改、删除。

### 默认权限

当安装了django.contrib.auth应用，对每个已安装的应用都会有三个默认许可--添加、修改、删除。

当运行manage.py migrate时，管理员对模型的默认权限就会被创建，包括已有的模型和新增的模型。

### web请求的认证

```
if request.user.is_authenticated:
    # Do something for authenticated users.
else:
    # Do something for anonymous users.


```
### 用户验证

```
from django.contrib.auth import authenticate, login
 
def my_view(request):
    username = request.POST['username']
    password = request.POST['password']
    user = authenticate(request, username=username, password=password)
    if user is not None:
        login(request, user)
        # Redirect to a success page.
    else:
        # Return an 'invalid login' error message.

```

### 用户登出

```

from django.contrib.auth import logout
 
def logout_view(request):
    logout(request)
    # Redirect to a success page.



```
### 重定向(login_required 装饰器)
作用：用户没有登录，跳回登录页面

```
from django.contrib.auth.decorators import login_required
 
@login_required(login_url='/login/')
def my_view(request):
    ...

```

### 重定向(LoginRequiredMixin)
作用：用户没有登录，跳回登录页面

```
from django.contrib.auth.mixins import LoginRequiredMixin
 
class MyView(LoginRequiredMixin, View):
    login_url = '/login/'
    redirect_field_name = 'redirect_to'


```

## 日志

### 介绍

Django使用的是Python的内置日志（logging）模块构建自己的日志系统

### 组成

python日志组成：
* Loggers（记录器）
* Handlers（处理器）
* Filters（过滤器）
* Formatters（格式化器）

#### Loggers 记录器
日志记录器是进入日志系统的入口点。每个日志记录器都是一个有名的‘桶’，将消息写入其中进行处理。 
记录器具有日志级别属性(log level)。这个日志级别描述日志记录器所处理消息的严重程度。Python定义的以下日志级别：
```
DEBUG：用于调试目的的低等级系统信息
INFO：一般的系统信息。
WARNING：描述发生的一个小问题的信息
ERROR:描述发生的一个大问题的信息
CRITICAL:描述已发生的关键问题的信息。
```
写入日志记录器的每个消息都是日志记录。每个日志记录都有一个日志级别，指示特定消息的严重程度。日志记录还可以包含描述正在记录的事件的有用元数据。这可能包括详细信息，例如堆栈跟踪或错误代码。 
当消息被提供给记录器时，将消息的日志级别与记录器的日志级别进行比较。如果消息的日志级别满足或超过记录器本身的日志级别，则消息将进行进一步处理。如果没有，消息将被忽略。 
一旦日志记录器确定需要处理消息，就会将其传递给处理程序。

#### Handlers
处理程序是决定日志记录器中每个消息处理行为的引擎。它描述特定的日志记录行为，例如向屏幕、文件或网络套接字写入消息。 
与日志记录器一样，处理程序也具有日志级别。如果日志记录的日志级别不满足或超过处理程序的级别，处理程序将忽略消息。 
日志记录器可以有多个处理程序，每个处理程序可以有不同的日志级别。通过这种方式，可以根据消息的重要性提供不同形式的通知。例如，您可以安装一个将错误和关键消息转发到分页服务的处理程序，而第二个处理程序将所有消息(包括错误和关键消息)记录到一个文件中，以便稍后进行分析。

#### Filters 
过滤器用于提供对日志记录从日志记录器传递到处理程序过程中的额外控制。 
默认情况下，满足日志级别要求的任何日志消息都将被处理。但是，通过安装过滤器，您可以在日志记录过程中添加额外的条件。例如，您可以安装一个过滤器，该过滤器只允许发出来自特定源的错误消息。 
过滤器还可以用于在发出日志记录之前修改日志记录。例如，您可以编写一个过滤器，如果满足特定的一组条件，则将错误日志记录降级为警告记录。 
过滤器可以安装在记录器或处理器上;可以在一个链中使用多个过滤器来执行多个过滤操作。

#### Formatters 
最终，需要将日志记录呈现为文本。格式化器描述了文本的精确格式。格式化程序通常由包含日志记录属性(LogRecord attributes)的Python格式化字符串组成;但是，您也可以编写自定义格式化程序来实现特定的格式化行为。

### 使用日志
在setting.py中

```
#导入模块
import logging
import django.utils.log
import logging.handlers
 
 
LOGGING = {
    'version': 1,
    'disable_existing_loggers': True,
    'formatters': {
       'standard': {
            'format': '%(asctime)s [%(threadName)s:%(thread)d] [%(name)s:%(lineno)d] [%(module)s:%(funcName)s] [%(levelname)s]- %(message)s'}  #日志格式 
    },
    'filters': {
    },
    'handlers': {
        'mail_admins': {
            'level': 'ERROR',
            'class': 'django.utils.log.AdminEmailHandler',
            'include_html': True,
        },
        'default': {
            'level':'DEBUG',
            'class':'logging.handlers.RotatingFileHandler',
            'filename': '/sourceDns/log/all.log',     #日志输出文件
            'maxBytes': 1024*1024*5,                  #文件大小 
            'backupCount': 5,                         #备份份数
            'formatter':'standard',                   #使用哪种formatters日志格式
        },
        'error': {
            'level':'ERROR',
            'class':'logging.handlers.RotatingFileHandler',
            'filename': '/sourceDns/log/error.log',
            'maxBytes':1024*1024*5,
            'backupCount': 5,
            'formatter':'standard',
        },
        'console':{
            'level': 'DEBUG',
            'class': 'logging.StreamHandler',
            'formatter': 'standard'
        },
        'request_handler': {
            'level':'DEBUG',
            'class':'logging.handlers.RotatingFileHandler',
            'filename': '/sourceDns/log/script.log', 
            'maxBytes': 1024*1024*5, 
            'backupCount': 5,
            'formatter':'standard',
        },
        'scprits_handler': {
            'level':'DEBUG',
            'class':'logging.handlers.RotatingFileHandler',
            'filename':'/sourceDns/log/script.log', 
            'maxBytes': 1024*1024*5, 
            'backupCount': 5,
            'formatter':'standard',
        }
    },
    'loggers': {
        'django': {
            'handlers': ['default', 'console'],
            'level': 'DEBUG',
            'propagate': False 
        },
        'django.request': {
            'handlers': ['request_handler'],
            'level': 'DEBUG',
            'propagate': False,
        },
        'scripts': { 
            'handlers': ['scprits_handler'],
            'level': 'INFO',
            'propagate': False
        },
        'sourceDns.webdns.views': {
            'handlers': ['default', 'error'],
            'level': 'DEBUG',
            'propagate': True
        },
        'sourceDns.webdns.util':{
            'handlers': ['error'],
            'level': 'ERROR',
            'propagate': True
        }
    } 
}

```

在views.py中使用

```
logger = logging.getLogger('sourceDns.webdns.views')    #刚才在setting.py中配置的logger
 
try:
    mysql= connectMysql('127.0.0.1', '3306', 'david')
except Exception,e:
            logger.error(e)        #直接将错误写入到日志文件

```

## 发送邮件



## 资讯聚合（RSS/Atom）

### 介绍
就本质而言，RSS和Atom是一种信息聚合的技术，都是为了提供一种更为方便、高效的互联网信息的发布和共享，用更少的时间分享更多的信息。

### 案例

## 分页

### 介绍

Django提供了一个新的类来帮助你管理分页数据，这个类存放在django/core/paginator.py.它可以接收列表、元组或其它可迭代的对象。

### 使用

```
from django.core.paginator import Paginator, EmptyPage, PageNotAnInteger
from django.shortcuts import render
 
def listing(request):
    contact_list = Contacts.objects.all()
    paginator = Paginator(contact_list, 25) 
    
    # Show 25 contacts per page
 
    page = request.GET.get('page')
    try:
        contacts = paginator.page(page)
    except PageNotAnInteger:
        # If page is not an integer, deliver first page.
        contacts = paginator.page(1)
    except EmptyPage:
        # If page is out of range (e.g. 9999), deliver last page of results.
        contacts = paginator.page(paginator.num_pages)
 
    return render(request, 'list.html', {'contacts': contacts})

```
```
{% for contact in contacts %}
    {# Each "contact" is a Contact model object. #}
    {{ contact.full_name|upper }}<br />
    ...
{% endfor %}
 
<div class="pagination">
    <span class="step-links">
        {% if contacts.has_previous %}
            <a href="?page={{ contacts.previous_page_number }}">previous</a>
        {% endif %}
 
        <span class="current">
            Page {{ contacts.number }} of {{ contacts.paginator.num_pages }}.
        </span>
 
        {% if contacts.has_next %}
            <a href="?page={{ contacts.next_page_number }}">next</a>
        {% endif %}
    </span>
</div>

```

## 消息框架

## 序列化

## 会话

## 站点地图

## 静态文件管理

## 数据验证

## 参考

[Django2.1 日志 中文文档](https://blog.csdn.net/qq_33666633/article/details/81953143)

[django 日志logging的配置以及处理](https://www.cnblogs.com/luohengstudy/p/6890395.html)

[Django之分页功能](https://www.cnblogs.com/kongzhagen/p/6640975.html)