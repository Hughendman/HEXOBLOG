---
title: node框架简要说明（express）
categories: node
tags:
  - node
  - koa2
keywords: node
abbrlink: 448baf37
date: 2018-06-09 11:15:00
---

# 最基础的两个后端框架（express和koa）

> 这两个是 Node.js 最基础的后端框架。因为太基础，所以构建一个 app 仍需要写很多脚手架代码，于是在它们基础之上出现了很多其他框架来减少编写这类代码。Express 应该是装机量最多的，而 Koa 更新一些, 使用的技术更新颖，例如 promises 和 async function，不再有回调函数嵌套的问题了。

### express

> 框架安装 ： cnpm install -g express@3
创建项目 : express -e httpsserver


> 安装 ： cnpm install express --save

> 以下三个插件最好一起和express安装

> body-parser - node.js 中间件，用于处理 JSON, Raw, Text 和 URL 编码的数据。（cnpm install body-parser --save）

> cookie-parser - 这就是一个解析Cookie的工具。通过req.cookies可以取到传过来的cookie，并把它们转成对象。（cnpm install cookie-parser --save）

> multer - node.js 中间件，用于处理 enctype="multipart/form-data"（设置表单的MIME编码）的表单数据。（cnpm install multer --save）

##### 以下是如何使用express插件(以下的案例是写了一个api为express的get请求的接口)

```
//express_demo.js 文件
var express = require('express');
var app = express();
 
app.get('/api/express', function (req, res) {
   res.send('Hello World');
})
 
var server = app.listen(8081, function () {
 
  var host = server.address().address
  var port = server.address().port
 
  console.log("地址为http://%s:%s", host, port)
 
})
```
###### 以上的req和res分别是请求和响应，想要获取具体的值和传递具体的值都需要通过他们


```
以下我会介绍他们（怒想看可以略过，本人认为许多属性用不到，可以直接看最后）
request 和 response 对象的具体介绍：

Request 对象 - request 对象表示 HTTP 请求，包含了请求查询字符串，参数，内容，HTTP 头部等属性。常见属性有：

req.app：当callback为外部文件时，用req.app访问express的实例
req.baseUrl：获取路由当前安装的URL路径
req.body / req.cookies：获得「请求主体」/ Cookies
req.fresh / req.stale：判断请求是否还「新鲜」
req.hostname / req.ip：获取主机名和IP地址
req.originalUrl：获取原始请求URL
req.params：获取路由的parameters
req.path：获取请求路径
req.protocol：获取协议类型
req.query：获取URL的查询参数串
req.route：获取当前匹配的路由
req.subdomains：获取子域名
req.accepts()：检查可接受的请求的文档类型
req.acceptsCharsets / req.acceptsEncodings / req.acceptsLanguages：返回指定字符集的第一个可接受字符编码
req.get()：获取指定的HTTP请求头
req.is()：判断请求头Content-Type的MIME类型
Response 对象 - response 对象表示 HTTP 响应，即在接收到请求时向客户端发送的 HTTP 响应数据。常见属性有：

res.app：同req.app一样
res.append()：追加指定HTTP头
res.set()在res.append()后将重置之前设置的头
res.cookie(name，value [，option])：设置Cookie
opition: domain / expires / httpOnly / maxAge / path / secure / signed
res.clearCookie()：清除Cookie
res.download()：传送指定路径的文件
res.get()：返回指定的HTTP头
res.json()：传送JSON响应
res.jsonp()：传送JSONP响应
res.location()：只设置响应的Location HTTP头，不设置状态码或者close response
res.redirect()：设置响应的Location HTTP头，并且设置状态码302
res.render(view,[locals],callback)：渲染一个view，同时向callback传递渲染后的字符串，如果在渲染过程中有错误发生next(err)将会被自动调用。callback将会被传入一个可能发生的错误以及渲染后的页面，这样就不会自动输出了。
res.send()：传送HTTP响应
res.sendFile(path [，options] [，fn])：传送指定路径的文件 -会自动根据文件extension设定Content-Type
res.set()：设置HTTP头，传入object可以一次设置多个头
res.status()：设置HTTP状态码
res.type()：设置Content-Type的MIME类型


```

>在上面这些属性中我主要使用了res.json(data)来跟前端传输数据，利用req.body来进行获取post方式传来后台的数据，利用req.baseUrl获取get方式传来后台的参数

>我主要使用的是express框架来构建的项目，下面的这个框架没有用过，但是他们两个都是一个团队开发的，但是koa最大的优点我认为是免除重复繁琐的回调函数嵌套

### koa

> 安装：npm i koa 

##### 现在来对上面的express以及现在的koa进行比较

> koa和express在表现上的一点不同是采用ctx一个参数来调用中间件，而不是express的req, res

> express的设计是串联的，设计思路超级简洁。
koa的某一个中间件可以自行选择之后中间件的执行位置的。

> express的社区还是大(很重要)。
koa本来就小，还被从koa1转koa2一折腾，就更小了。

> 因为没有实际开发过所以介绍到此为止，个人认为以后的发展还是koa2比较有优势
