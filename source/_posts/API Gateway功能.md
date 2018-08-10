---
title: API Gateway功能
categories: 微服务
tags: 微服务
keywords: 微服务
abbrlink: b8717e5
date: 2018-07-13 19:15:00
---

前面两篇从其他地方抄来的文章主要是介绍以下微服务以及API Gateway ，以下就是我实现的API Gateway主要的功能以及实现的思路

## 技术选型

由于本人技术限制，大家已经猜到了

node（koa2） + mysql + VUE + nginx

前端和mysql没什么好说的，纯粹是我最熟悉就是他们两个了

对于大多数应用程序而言，API网关的性能和可扩展性通常都非常重要。因此，将 API网关构建在一个支持异步、I/O非阻塞的平台上是合理的。有多种不同的技术可以用于实现一个可扩展的API网关。在JVM上，可以使用一种基于 NIO的框架，比如Netty、Vertx、Spring Reactor或JBoss Undertow中的一种。一个非常流行的非JVM选项是Node.js，它是一个以Chrome JavaScript引擎为基础构建的平台。

### 功能实现

#### API网关作为微服务的入口点

这是API网关的基本功能

为了实现这个功能，我需要有一个具体的方案

因为gs的需求，我们只有http或者https两种，其他的协议方式可以留到以后进行优化

##### 一般分为两种get/post请求方式

###### get方式

参数一： url

参数二：参数：query

参数三：gateway 地址

###### post方式

参数一： url

参数二：参数：props

参数三：gateway 地址

##### 技术实现

```
npm install --save request

```

get

```
const request = require('request');
request('您的请求url', function (error, response, body) {
  if (!error && response.statusCode == 200) {
    console.log(body) // 请求成功的处理逻辑
  }
});

```


post



```

const request = require('request');
let url="请求url";
let requestData="参数对象";
request({
    url: url,
    method: "POST",
    json: true,
    headers: {
        "content-type": "application/json",
    },
    body: JSON.stringify(requestData)
}, function(error, response, body) {
    if (!error && response.statusCode == 200) {
        console.log(body) // 请求成功的处理逻辑
    }
}); 


```

post form 

```
const request = require('request');
let url="请求url";
let formData="参数对象";

request.post({url:url, form:formData}, function(error, response, body) {
    if (!error && response.statusCode == 200) {
       console.log(body) // 请求成功的处理逻辑  
    }
})

```

###### 匹配路由

（原方案是动态生成路由，不能满足方案，已放弃）

利用正则模糊匹配，根据参数决定具体的逻辑


匹配任何字符：[\s\S]*

没有成功

```

const router = require('koa-router')()
router.prefix('/api')
let arr = [];
let obj = "";
for(let i=0;i<20;i++){
	obj = obj + "/:id";
	arr.push(obj);
}

arr.forEach(function(item,index){
	router.get(item,async function (ctx, next) {
	  let data;
	  ctx.body = 'this is a users response!'
	})
});


module.exports = router


```

#### 认证

使用access_token

每一个url根据用户生成一个assess_token，状态码为1（true）或者0（false）

```

access_token = md5('path名称'+'日期'+'方法名'+'模块名'+'随机字符串')

```

#### 数据聚合

可能是两个接口的数据的聚合

![](https://raw.githubusercontent.com/Hughendman/picture/master/API/10.png)

#### 熔断

token认证中的状态码控制

#### 监控

log4js日志收集响应日志，通过响应日志监控

每天会将日志导入到数据库中