
---
title: node 和 模板引擎（jade）
date: 2018-06-09 11:15:00
categories : node
tags: node
keywords : node
---
> 注意：在node中假如同一路径有两个方法来对其进行处理，那么只有匹配到的第一个方法会被执行，剩余的略过

> 为了解决这一问题，node
使用了 next

#### 具体用法

- index.js
```
router.get('/base/pageA', function (req, res, next) {  
    res.send('index.js.');  
    next();  
});  

```

- base.js
```
router.get('/pageA', function (req, res) {  
    //res.send('base.js!');  
    console.log("base.js")  
})  

```


#### 优点

1. 轻易的实现中间件

由

```
graph LR;  
　　A-->c;   
　　
```
变为

```
graph LR;  
　　A-->B;   
　　B-->c;  
　　
```

2. 提高代码的复用性