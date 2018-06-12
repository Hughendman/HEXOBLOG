---
title: node 和 模板引擎（jade）
date: 2018-06-09 11:15:00
categories : node
tags: 
    - node
    - koa2
keywords : node
---

> express 框架里内嵌了Jade模板引擎。

#### 安装

> npm install jade –global


#### 执行

> 创建一个 index.jade 的文件，
然后执行 jade index.jade

> 这样做就会将jade文件转换为html文件

### jade还有一些参数

> jade -P index.jade

> 这样编辑出来的index.html文件是没有进行过压缩的

> jade -P -w index.jade

> -w 只要保存就自动编辑

> jade -P -w sample.jade -O sample.json

> -O 用来给jade文件传递对象或JSON文件，用来替换模板内的变量

#### 语法

编辑前
```
doctype html
html
  head
  body
    h1.titleClass#titleID My First Jade Page

```
编辑后
```
<!DOCTYPE html>
<html>
  <head></head>
  <body>
    <h1 id="titleID" class="titleClass">My First Jade Page</h1>
  </body>
</html>

```





