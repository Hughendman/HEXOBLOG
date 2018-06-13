---
title: node--require（）为什么是同步的
categories: node
tags:
  - node
  - 理论
keywords: node
abbrlink: 82a3d2fb
date: 2018-06-09 11:15:00
---

1、因为使用的是CommonJS标准


2、作为公共依赖的模块，自然要异步加载到位


3、模块的个数往往有限制，Node会自动缓存已经加载的模块，再加上访问的都是本地文件，产生的IO开销可以忽略不计


4、Node运行在服务器端，很少遇见需要频繁重启服务的情况。服务启动时候画上几秒也没有关系