---
title: node中的next()用法
categories: node
tags: node
keywords: node
abbrlink: '2971356'
date: 2018-06-09 11:15:00
---

next()函数是用来调用下一个中间件的。


什么是中间件：位于中间，为两侧提供服务的软件


举个例子

```
app.use((ctx,next)=>{
    ctx.name = 'yixns',
    next();
})
app.use((ctx,next)=>{
    ctx.age = '24',
    next();
})
app.use((ctx,next)=>{
    console.log(`${ctx.name) is ${ctx.age} years old.`)
    next();
})
app.go({});



```
ctx 参数就是 app.go 接受的对象。调用 app.go 其实会调用目标函数 app.callback，但是调用 app.callback 之前我们可以先让参数 ctx 通过一系列的中间件，最后才会传递给 app.callback。

使用 app.use 插入任意中间件，中间件是一个函数，可以被传入一个 ctx 和 next；调用 next 的时候会执行下一个中间件。如果不调用 next 会阻止接下来所有的中间件的执行，也不会执行 app.callback。


简单的来说，中间件就是流水线，流水线徐亚统一接口进行工序对接，而这个借接口，就是next

[koa2中间件](http://www.cnblogs.com/cloud-/p/7239819.html)


