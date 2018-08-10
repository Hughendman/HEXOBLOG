---
title: python-nginx日志埋点导入mysql
categories: python
tags:
  - python
  - 埋点
keywords: python
abbrlink: dccb0cee
date: 2018-06-28 11:15:00
---

前不久发了一篇关于nginx日志埋点的文章，这篇文章是讲的如何利用python将nginx日志的数据导入到mysql库中，之后的操作将交给数据分析师来进行

### 前置

需要python版本为2.7

需要下载pip： easy_install pip

需要下载MySQLdb： pip install MySQL-python

如果还有下载不懂的可以查看我的这篇文章--python 闲散知识

注意： 这个文件要放在nginx的logs文件下面执行

### 代码

```
# encoding: utf-8

import datetime
import os
import MySQLdb


db = MySQLdb.connect("localhost","userName","pwd","",charset='utf8' )


today = datetime.date.today()
currentday0 = today - datetime.timedelta(days=0)
targe = currentday0.strftime("%Y%m")

cursor = db.cursor()



def insertdata (num):
	for index in range(num):
		tag1 = index + 1
		currentday = today - datetime.timedelta(days=tag1)
		day = currentday.strftime("%d")
		catalogue = "access_"+day+".log"
		if os.path.exists(targe + '/' + catalogue):
			f = open( targe + '/' + catalogue,'r')
			str = f.read()
			f.close()
			arr = str.split('\n')
			for index_i in range(len(arr)):
			    # 因为有非常多的项目利用nginx做了代理，所以这里进行一次过滤
				if len(arr[index_i].split('^A')) >= 17:
					if arr[index_i].split('^A')[15] == '这里写的是你的pid':
						list_arr = arr[index_i].split('“')[1].split('”')[0]
						list_data = list_arr.split('^A')
					
						li = '","'.join(list_data)
						
						sql = 'INSERT INTO tj_md(msec,remote_addr,u_domain,u_url,u_title,u_referrer,u_sh,u_sw,u_cd,u_lang,http_user_agent,u_utrace,u_account,time_local,u_uid,u_pid,u_option) VALUES ("' + li + '")'
				
						try:
						   cursor.execute(sql)
						   db.commit()
						except:
						   db.rollback()

# 这里面填写的数字代表的是你要执行多少天的操作，第一次执行的我设置为昨天的数据
insertdata(1)


db.close()

```

### 添加定时任务

因为添加了定时任务（每天执行一次），所以函数中添加的参数为1

```
insertdata(1)

```

crontab -e  后添加，每天凌晨四点执行一次

```
0 4 * * * python /usr/logcal/nginx/logs/1.py

```
如果权限不过请执行：
chmod a+x /usr/local/nginx/logs/1.py





