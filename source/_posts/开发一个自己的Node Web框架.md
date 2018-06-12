---
title: 开发一个自己的Node Web框架
date: 2018-06-09 11:15:00
categories : node
tags: [node,理论]
keywords : node
---

## 前言
学习一下怎么开发框架，对express和koa2的使用会让你更加得心应手。本文借鉴李锴的文章

## 框架的雏形

首先构建一个application.js，主要的方法都会在这里声明，构建大致的代码框架

application.js

```
const Emitter = require('events');
const http = require("http");

module.exports = class Loa extends Emitter {
    constructor(){
        super();
        this.middleware= [];
    }

    use(fn){

    }
    //middle 是中间件的例子
    middle(req,res) {
        res.end("server start");
    }
    listen(port) {
        console.log(port+" port is start")
        const server = http.createServer((req,res) => {
            this.middle(req,res);
        });
        return server.listen(port);
    }
};

```
main.js中引用

```
const Loa = require("./application");

const app = new Loa();
app.listen(8000);

```

## 框架的完善

写不下去了，我在代码中把注释写的非常明白。



















