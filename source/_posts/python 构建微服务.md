---
title: python 构建微服务
categories: python
tags: python
keywords: python
abbrlink: f3250ef9
date: 2018-09-12 16:15:00
---

# python构建微服务

## 方案一

### 架构

Nameko + API Swagger

### 简介

Nameko是一个让python程序员关注应用逻辑和测试的微服务框架。

### 项目

```
py -2 -m pip install nameko
py -2 -m pip install yagmail
cd service 
nameko run service --broker amqp://guest:guest@localhost

```

```
py -2 -m pip install nameko
py -2 -m pip install flask
py -2 -m pip install flasgger
git clone https://github.com/rochacbruno/nameko-example.git
cd api
python2 api.py

```
打开浏览器 

http://localhost:5000/apidocs/index.html 

[git 案例 nameko-example](https://github.com/rochacbruno/nameko-example)

#### rabbitMQ

[安装-windows下 安装 rabbitMQ 及操作常用命令](https://www.cnblogs.com/ericli-ericli/p/5902270.html)

安装成功后访问：http://localhost:15672/

rabbitMQ新增用户
```
rabbitmqctl.bat add_user username password

```
rabbitMQ查询用户
```
rabbitmqctl.bat add_user username password

```
给用户超级管理员角色
```
rabbitmqctl.bat set_user_tags username administrator
```
修改密码
```
rabbitmqctl change_password userName newPassword
```
删除用户
```
rabbitmqctl.bat delete_user username
```

权限相关命令
```
## 设置用户权限

rabbitmqctl  set_permissions  -p  VHostPath  User  ConfP  WriteP  ReadP

## 查看(指定hostpath)所有用户的权限信息

rabbitmqctl  list_permissions  [-p  VHostPath]

## 查看指定用户的权限信息

rabbitmqctl  list_user_permissions  User

##  清除用户的权限信息

rabbitmqctl  clear_permissions  [-p VHostPath]  User

```
#### docker



[docker安装教程](http://www.runoob.com/docker/windows-docker-install.html)

安装完成后记得配置环境变量

#### docker-compose

```
py -2 -m pip install docker-compose

```

#### flask

Flask是一个使用 Python 编写的轻量级 Web 应用框架。其 WSGI 工具箱采用 Werkzeug ，模板引擎则使用 Jinja2 。Flask使用 BSD 授权。
Flask也被称为 “microframework” ，因为它使用简单的核心，用 extension 增加其他功能。Flask没有默认使用的数据库、窗体验证工具。(来自百度)

##### 初次使用

flask_test.py
```
from flask import Flask
app = Flask(__name__)
@app.route("/")
def hello():    
    return "Hello World!"
 
if __name__ == "__main__":
    app.run()

```
执行
```
py -2 -m pip install flask

python2 flask_test.py

```
打开 http://localhost:5000/

#### flasgger

flasgger为在flask框架中使用的swagger，flasgger是flask支持的swagger UI，便于调试使用flask框架搭建的web api接口

[参考文章](https://changsiyuan.github.io/2017/05/20/2017-5-20-flasgger/)

##### 安装

```
py -2 -m pip install flasgger

```
##### 案例

上面的http://localhost:5000/apidocs/index.html 即为案例

##### 配置解析

```
@app.route('/compute', methods=['POST'])
@swag_from('compute.yml')

```

compute.yml

```
Micro Service Based Compute and Mail API
This API is made with Flask, Flasgger and Nameko
---
parameters:
  - name: body
    in: body
    required: true
    schema:
      id: data
      properties:
        operation:
          type: string
          enum:
            - sum
            - mul
            - sub
            - div
        email:
          type: string
        value:
          type: integer
        other:
          type: integer
responses:
  200:
    description: |
       Please wait the calculation, you'll receive an email with results

```

###### parameters :参数配置
    GET方法：放置url中
    POST方法：放置在schema
    
###### responses :返回信息
    
在这里介绍一篇文章，[API文档自动生成工具apiDoc](https://hughendman.github.io/post/652bf3bd.html)
## google-api-python-client

谷歌正式推出Python版Google API客户端库

[git地址](https://github.com/googleapis/google-api-python-client)


谷歌称，如果开发者想使用Google API构建一个Python应用程序，那么强烈建议使用该客户端库，因为该库可以帮助开发者轻松调用任何RESTful Google API，并抓取返回的数据。此外，该库还可以帮助开发者处理OAuth 2.0验证协议以及所有的错误，而无需写额外的代码。 ([来自网络](http://www.iteye.com/news/26053))

### 使用

```
pip install --upgrade google-api-python-client

```

### 版本支持

Python Version

Python 2.7, 3.4, 3.5, and 3.6 are fully supported and tested. (来自官网)

## pip网络问题解决

由于网络原因，需要更改源

```
pip install xlrd http://pypi.douban.com/simple/ --trusted-host pypi.douban.com

```

永久解决方案：

在pip安装目录（C:\Python27\pip-9.0.1\pip-9.0.1  这是我的目录）创建（.py）文件

```
import os 
 
ini="""[global]
index-url = https://pypi.doubanio.com/simple/
[install]
trusted-host=pypi.doubanio.com
""" 
pippath=os.environ["USERPROFILE"]+"\\pip\\" 
 
if not os.path.exists(pippath): 
    os.mkdir(pippath) 
 
with open(pippath+"pip.ini","w+") as f: 
    f.write(ini) 

```
执行，可以解决问题


## 兼容python2.7和python3（window版本）

因为电脑中已经有版本2

进入：C:\Python27 安装目录下面

将python.exe改为python2.exe

执行： python2 -V 查看是否成功

同理 python 3也是同样操作，安装成功后改为python3.exe（默认安装目录：C:\Users\admin\AppData\Local\Programs\Python\Python37）

执行： python3 -V 查看是否成功

加上以下也可以 py 执行
```
#! python2 or #! python3

```

pip使用如下：

```

py -2 -m pip install XXXX

py -3 -m pip install XXXX

```
## pip升级

```
python -m pip install --upgrade pip

```

## 方案二Sanic

[模板来源-微服务Sanic制作一个简易本地restful API](https://blog.csdn.net/sinat_26917383/article/details/79292506)

Sanic是一个支持 async/await 语法的异步无阻塞框架，Flask的升级版，效率更高，性能会提升不少


```
# coding:utf-8
 
from sanic import Sanic
from sanic.response import text

app = Sanic()


@app.route("/")
async def test(request):
    return text('Hello World!')


if __name__ == "__main__":
    app.run(host="0.0.0.0", port=8000)
```

执行

```
py -3 -m pip install Sanic
## 如果报错，执行
py -3 -m pip install --upgrade pip
## https://pypi.org/project/Sanic/#files 下载whl文件
py -3 -m pip install C:\Users\admin\Downloads\httptools-0.0.11.tar.gz
C:\Users\admin\Downloads\sanic-0.8.1-py3-none-any.whl

```

