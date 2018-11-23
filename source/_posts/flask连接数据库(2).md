---
title: flask连接数据库(2)
categories:
  - flask
  - python
tags:
  - flask
  - python
keywords:
  - flask
  - python
abbrlink: b65ff4ad
date: 2018-10-17 20:15:00
---

### 数据库类型

| 数据库 | 命令 |  
| ------ | ------ | 
| MySQL | pip install PyMySQL |
| Postgres | pip install psycopg2 | 
| Presto | pip install pyhive | 
| Oracle | pip install cx_Oracle | 
| sqlite | 短文本 | 
| Redshift | pip install sqlalchemy-redshift | 
| MSSQL | pip install pymssql | 
| Impala | pip install impyla | 
| SparkSQL | pip install pyhive | 

### mysql

pip install PyMySQL

```
#!/usr/bin/python3
 
import pymysql
 
# 打开数据库连接
db = pymysql.connect("localhost","testuser","test123","TESTDB" )
 
# 使用 cursor() 方法创建一个游标对象 cursor
cursor = db.cursor()
 
# 使用 execute()  方法执行 SQL 查询 
cursor.execute("SELECT VERSION()")
 
# 使用 fetchone() 方法获取单条数据.
data = cursor.fetchone()
 
print ("Database version : %s " % data)
 
# 关闭数据库连接
db.close()

```
插入
```

#!/usr/bin/python3
 
import pymysql
 
# 打开数据库连接
db = pymysql.connect("localhost","testuser","test123","TESTDB" )
 
# 使用cursor()方法获取操作游标 
cursor = db.cursor()
 
# SQL 插入语句
sql = """INSERT INTO EMPLOYEE(FIRST_NAME,
         LAST_NAME, AGE, SEX, INCOME)
         VALUES ('Mac', 'Mohan', 20, 'M', 2000)"""
try:
   # 执行sql语句
   cursor.execute(sql)
   # 提交到数据库执行
   db.commit()
except:
   # 如果发生错误则回滚
   db.rollback()
 
# 关闭数据库连接
db.close()

```

查询

```
#!/usr/bin/python3
 
import pymysql
 
# 打开数据库连接
db = pymysql.connect("localhost","testuser","test123","TESTDB" )
 
# 使用cursor()方法获取操作游标 
cursor = db.cursor()
 
# SQL 查询语句
sql = "SELECT * FROM EMPLOYEE \
       WHERE INCOME > '%d'" % (1000)
try:
   # 执行SQL语句
   cursor.execute(sql)
   # 获取所有记录列表
   results = cursor.fetchall()
   for row in results:
      fname = row[0]
      lname = row[1]
      age = row[2]
      sex = row[3]
      income = row[4]
       # 打印结果
      print ("fname=%s,lname=%s,age=%d,sex=%s,income=%d" % \
             (fname, lname, age, sex, income ))
except:
   print ("Error: unable to fetch data")
 
# 关闭数据库连接
db.close()

```

### Postgres

```
import psycopg2
import psycopg2.extras
conn = psycopg2.connect(host=’localhost’, port=port, user=’root’, password=’abcd234’, database=’database’)
cursor = conn.cursor(cursor_factory=psycopg2.extras.DictCursor)
#执行SQL脚本 
cursor.execute(‘SELECT * FROM test WHERE id > %s;’, (5,)) 
#查看生成的sql脚本
cursor.mogrify(‘SELECT * FROM test WHERE a = %s AND b = %s;’, (‘a’, ‘b’))
#插入数据
cursor.execute(‘INSERT INTO test (a, b) VALUES (%s, %s) RETURNING id;’, (‘a’, ‘b’))
item = cursor.fetchone()
print item[0] #这里就是刚才插入的记录的ID
```


### flask-sqlalchemy

Flask拥有丰富的扩展组件，数据库管理方面Flask-SQLAlchemy简化了数据库管理的操作

```
pip install flask-sqlalchemy

```

在以后的文章中我还会继续介绍