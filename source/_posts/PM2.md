---
title: PM2
categories: koa2
tags: koa2
keywords: koa2
abbrlink: 1efd976a
date: 2018-07-11 16:15:00
---

## 简介

pm2 是一个带有负载均衡功能的Node应用的进程管理器.

## 使用

### 安装

```
npm install -g pm2 
```

### 启动项目

方式一：

```
pm2 start app.js

```
方式二：

我使用的koa2的cli所以我启动项目的时候使用的命令是：

```
npm run prd
```

### 显示所以进程的状态

```
pm2 list
```
在这里你会找到你项目的id，很有用，下面的命令会用到这个id

### 删除这个进程

```
pm2 delete ${id}

```

### 重启项目

```
pm2 restart all  //重启所有

pm2 restart ${id}  //重启这个id的项目

```

### 暂停项目

```

pm2 restart all  //暂停所有

pm2 restart ${id}  //暂停这个id的项目

```

### 查看项目具体信息

由于有的时候项目非常多，已经不知道哪个是哪个，所以可以在这里面查看到项目的路径

```
pm2 describe ${id}  //暂停这个id的项目

```

### 查看日志

如果你有打印端口号的习惯，那么你可以通过这种方式查看到这个项目的端口号

```
pm2 logs //查看所有日志

pm2 logs ${id}  //查看当前id的日志

```