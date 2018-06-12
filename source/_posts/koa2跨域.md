---
title: koa2跨域
date: 2018-06-09 11:15:00
categories : node
tags: 
    - node
    - koa2
keywords : node
---

"koa2-cors": "^2.0.5",


```
const cors = require('koa2-cors');

app.use(cors({
    origin: function(ctx) {
        if (ctx.url === '/test') {
            return false;
        }
        return '*';
    },
    exposeHeaders: ['WWW-Authenticate', 'Server-Authorization'],
    maxAge: 5,
    credentials: true,
    allowMethods: ['GET', 'POST', 'DELETE'],
    allowHeaders: ['Content-Type', 'Authorization', 'Accept'],
}));

```