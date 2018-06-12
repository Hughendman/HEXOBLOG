---
title: node（koa2）快速创建项目
date: 2018-06-09 11:15:00
categories : node
tags: 
    - node
    - koa2
keywords : node
---

# 创建项目（但是不支持热加载）

```
npm install -g koa-generator   //全局安装

```

### 一键创建项目

```
koa2 koa2-test

cd koa2-test

npm i
```

### 执行

```
npm run dev 
```
> 这里可能报错，原因是因为需要安装bable

```
npm install --save-dev babel-core babel-polyfill babel-preset-es2015 babel-preset-stage-3
```

> 然后在bin/www中使用bable

```
require("babel-polyfill");
require('babel-core/register')({
    presets: ['es2015', 'stage-3']
});
```

### 如何写接口

#### 在routes文件夹下创建一个js文件，我们可以起一个yinxs,代码如下，我写了三个关于get的接口,以及一个post接口
```
const router = require('koa-router')()

router.prefix('/yinxs')

router.get('/yinxs', function (ctx, next) {
    ctx.body = 'I am yinxs!'
})

router.get('/father', function (ctx, next) {
    ctx.body = 'I am father'
})

router.get('/son', function (ctx, next) {
    ctx.body = 'I am son'
})

router.post('/getname', function(ctx, next) {
    console.log(JSON.stringify(ctx.request));
    ctx.body = 'I am yinxs post'
})

module.exports = router


```
> 在app.js将新写的接口关联进去

```
const yinxs = require('./routes/yinxs')
app.use(yinxs.routes(), yinxs.allowedMethods())

```

> 执行npm run start
访问 /yinxs/yinxs   /yinxs/son  /yinxs/father  可以获取到数据


注：项目启动的是bin目录下面的www，而不是app.js

[koa2跨域](https://github.com/zadzbw/koa2-cors)




