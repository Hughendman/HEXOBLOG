---
title: python27常用
categories: python
tags: python
keywords: python
abbrlink: afab67f
date: 2018-07-03 11:15:00
---

## 中文编码

```
# encoding: utf-8

```

Python中默认的编码格式是 ASCII 格式，在没修改编码格式时无法正确打印汉字

## 基础语法

#### 条件语句

```
if 判断条件1:
    执行语句1……
elif 判断条件2:
    执行语句2……
elif 判断条件3:
    执行语句3……
else:
    执行语句4……if 判断条件：
    执行语句……
else：
    执行语句……

```
#### 循环语句

```
a=[1,2,3]
for i in a:
   print('当前数组 :', i)
 
```

```

a=[1,2,3]
for i in range(len(a)):
    print(a[i])

```

#### 函数

```
def functionname( parameters ):
   function_suite
   return [expression]
   
```

#### 文件

读文件

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
# 打开一个文件
fo = open("foo.txt", "r+")
str = fo.read(10)
print "读取的字符串是 : ", str
# 关闭打开的文件
fo.close()

```

写文件

```
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
# 打开一个文件
fo = open("foo.txt", "w")
fo.write( "www.runoob.com!\nVery good site!\n")
 
# 关闭打开的文件
fo.close()

```

判断文件是否存在

```
import os

if os.path.exists(response_name):
    print('文件存在')
else:
    print('文件不存在')

```

### 其他应用

#### excel

安装xlrd、xlwt、xlutils
xlrd：是python从excel读数据的第三方控件；
xlwt：是python从excel写数据的第三方控件；
xlutils：是python使用xlrd、xlwt的工具箱。若安装不成功，可能原因是需要安装setuptools。

[root@vm4 python]# pip install xlrd

[root@vm4 python]# pip install xlwt

[root@vm4 python]# pip install xlutils

语法如下：

```

import xlwt

workbook = xlwt.Workbook(encoding = 'utf-8')

worksheet = workbook.add_sheet('My Worksheet')

# excel当前格大小
worksheet.col(0).width = 256*20
worksheet.col(1).width = 256*20
worksheet.col(2).width = 256*20
# 第一行
worksheet.col(0).width = 256*20
worksheet.write(0, 0, label = '日期')
worksheet.write(0, 1, label = 'PV')
worksheet.write(0, 2, label = 'UV')
# 第二行
worksheet.write(1, 0, label = '2018/7/3')
worksheet.write(1, 1, label = 3)
worksheet.write(1, 2, label = 4)

# 生成excel文件
workbook.save('excel.xls')

```

#### 日期

```
import datetime

today = datetime.date.today()
yesterday = today - datetime.timedelta(days=1)

day = yesterday.strftime("%Y-%m-%d")

```
#### 数据库操作
先下载改模块：
pip install MySQLdb

```
import MySQLdb

db = MySQLdb.connect("localhost","root","abcd","databaseName",charset='utf8' )

cursor = db.cursor()

sql = 'SELECT * FROM tj_md'

try:
   cursor.execute(sql)
   results = cursor.fetchall()
   for row in results:
   print(row)

except:
   print "Error: unable to fecth data"

db.close()

```