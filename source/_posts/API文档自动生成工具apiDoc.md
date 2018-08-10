---
title: API文档自动生成工具apiDoc
categories: node
tags: node
keywords: node
abbrlink: 652bf3bd
date: 2018-07-19 18:15:00
---

最近写项目的时候不打算一边写接口一边写前端，感觉这样写很混乱，所以打算先把后端的接口完成，前端先放在后面，但是写完了接口时经常发现自己忘记写了哪些，所以要写一个API的文档，下面我来介绍一个非常方便的工具，API文档自动生成工具apiDoc。

## 安装

```
npm install apidoc -g

```
执行
```
apidoc -v

```
出现：

```
warn: Please create an apidoc.json configuration file.
verbose: apidoc-generator name: apidoc
verbose: apidoc-generator version: 0.17.6
verbose: apidoc-core version: 0.8.3
verbose: apidoc-spec version: 0.3.0
verbose: run parser

```
安装成功

## 生成文档

在目录里面放一个apidoc.json

```
{
  "name": "Apidoc Example",
  "version": "1.0.0",
  "description": "Apidoc Example descrption",
  "title": "Custom apiDoc browser title",
  "url" : "http://localhost:8000/api/v1",
  "sampleUrl": "http://localhost:8000/api/v1",
  "template": {
    "withCompare": true,
    "withGenerator": true
  }
}

```

我把路由全部放在routes里面，生成的apidoc直接打开index.html就可以访问
```
apidoc -i routes/ -o apidoc/ # 可以通过搜索routes目录中的文件快速的生成文档文件，并将这些文件放在apidoc目录下。

```

## 注释格式

你需要写特定注释才可以生成。

举个例子

```
/**
 *
 * @api {post get} /{model}/query  查询数据
 * @apiName query
 * @apiGroup 业务单元基础
 * @apiVersion 1.0.0
 * @apiDescription 接口详细描述
 *
 * @apiParam {Object} body 请求参数JSON对象
 *
 * @apiSuccess {String} status 结果码
 * @apiSuccess {String} message 消息说明
 * @apiSuccessExample Success-Response:
 *  HTTP/1.1 200 OK
 *  {
     *    code:1,
     *    message:'success',
     *    data:{total:100, result:[]}
     *   }
 *  @apiErrorExample {json} Error-Response:
 *  HTTP/1.1 200
 *  {
     *     code:0,
     *     message:'user not found',
     *   }
 *   @apiPermission Auth
 */

```

## 展示

![](https://raw.githubusercontent.com/Hughendman/picture/master/apidoc/1.png)