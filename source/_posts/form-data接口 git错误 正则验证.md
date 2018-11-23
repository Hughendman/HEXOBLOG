---
title: form-data接口 git错误 正则验证
categories: node
tags: node
keywords: node
abbrlink: f9641aa3
date: 2018-09-05 16:15:00
---

## koa2 formdata接口

今天在尝试项目
koa-bodyparser 不解析 form-data接口传过来的参数

在网上查了一下解决方案

```
cnpm i --save koa-body

```

```

const koaBody = require('koa-body');

app.use(koaBody({multipart: true}));

```

这样就可以解析form-data

## git问题

报错：error: RPC failed; HTTP 411 curl 22 The requested URL returned error: 411 Length Required

解决：git config http.postBuffer  524288000

## 验证邮箱以及电话的正则
邮箱
```
module.exports = function checkMobile(s){
    let regu = /^[a-zA-Z0-9_.-]+@[a-zA-Z0-9-]+(\.[a-zA-Z0-9-]+)*\.[a-zA-Z0-9]{2,6}$/;
    let re = new RegExp(regu);
    if (re.test(s)) {
        return true;
    }else{
        return false;
    }
};

```
电话（包含电话以及手机号码）
```
module.exports = function checkMobile(s){
    let regu1 =  /^((1)+\d{10})$/;
    let regu2 =  /(^(\d{3,4}-)?\d{6,8}$)|(^(\d{3,4}-)?\d{6,8}(-\d{1,5})?$)/;
    let re1 = new RegExp(regu1);
    let re2 = new RegExp(regu2);
    if (re1.test(s)||re2.test(s)) {
        return true;
    }else{
        return false;
    }
};

```