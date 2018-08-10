---
title: python 闲散知识
categories: python
tags: python
keywords: python
abbrlink: 73c670e0
date: 2018-06-26 11:15:00
---

### 安装

[python升级2.7版本](https://www.cnblogs.com/Edwardzhao/p/5856924.html)


### BUG

#### 1

> pkg_resources.DistributionNotFound: The 'pip==7.1.0' distribution was not found and is

解决方案：

1. 使用easy_install pip==7.1.0 
2. 然后再安装easy_install pip==9.0.1 
3. 然后删掉rm -f /usr/local/lib/python2.7/site-packages/pip-7.1.0-py2.7.egg 

#### 2

> cannot concatenate 'str' and 'list' objects


解决：

列表不能直接和字符串连接在一起



#### 3

> _mysql_exceptions.OperationalError: (2006, 'MySQL server has gone away')

修改了一下程序，当日志分析完毕后再打开与mysql数据库的链接，且当使用完毕后关闭链接

#### 4

> list index out of range

第1种可能情况 
list[index]index超出范围

第2种可能情况 
list是一个空的 没有一个元素 
进行list[0]就会出现该错误


### python用法

#### 定时执行python脚本




#### python 获取当前日期

```
import datetime
today = datetime.date.today()

```

#### python 获取前几天日期

```
import datetime
today = datetime.date.today()

yesterday = today - datetime.timedelta(days=1)
day = yesterday.strftime("%Y-%m-%d")

```

#### python 操作数据库

> pip install MySQL-python





### 语法

#### 数组合并字符串

```
",".join(arr)

```

