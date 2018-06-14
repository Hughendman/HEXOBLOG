---
title: react中的dva的使用
categories: react
tags: react
keywords: react
abbrlink: 3fb51549
date: 2018-06-14 11:15:00
---

## 参考

[参考](http://blog.csdn.net/qq_31655965/article/details/72716557)

## dva介绍

> 框架: dva是个框架，集成了redux、redux-saga、react-router-redux、react-router快速初始化: 可以快速实现项目的初始化，不需要繁琐地配置简化开发：将initState、saga、reducer集成到一个model里面统一管理，避免文件散落在各个文件里面，便于快速查找与开发简洁的API：整个项目中只有dva、app.model、app.router、app.use、app.start几个API无缝对接：跟react的生态没有冲突，例如可以直接使用redux devtool工具动态机制：app.start以后，仍然可以注册model，灵活性较高

## 安装dva

> npm install dva-cli -g

## 使用

> dva new react-dva

## dva 的命令

[常用命令](http://www.chenfangka.com/?p=1252)

> 生成路由： dva g route users

> 生成model: dva g model users

> 生成组件： dva g component Users/Users

> 增加service

```
import request from '../utils/request';
// export function fetch({ page = 1 }) {
//   return request(`/api/users?_page=${page}&_limit=5`);
// }

```

## 启动

> npm start
> 80端口可以启用

## 使用的框架

[antd](https://ant.design/docs/react/introduce-cn)

> 使用 ：  npm install antd-init -g

> babel-plugin-import 是用来按需加载 antd 的脚本和样式的

> npm install antd babel-plugin-import --save

## 注释：nvm alias defalut v6.10.0 可以设置ndoe的默认版本


