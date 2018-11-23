---
title: node中webservice的使用
categories: node
tags: node
keywords: node
abbrlink: 81a0d70b
date: 2018-08-16 16:15:00
---


### easy-soap-request

#### 下载

```
cnpm i --save easy-soap-request

```

#### 使用

```
const soapRequest = require('easy-soap-request')


let response= await soapRequest(url, headers,xml);//url是地址，headers是请求头，xml是参数


```

#### 源码修改

在index.js中statusCode的参数改为response

这样你可以获取包括头的所有信息

### node-koa2 支持xml

#### 下载

```
cnpm i --save koa-xml-body

```

添加
```
app.use(xmlParser());//要放在bodyparser之前
app.use(bodyparser({
  enableTypes:['json', 'form', 'text']
}));

```

### xml与json互相转换

#### 下载

```
cnpm i --save xml2js

```

#### 使用

```
const xml2js = require('xml2js');

const xmlBuilder = new xml2js.Parser();
const jsonBuilder = new xml2js.Builder();

let xml = jsonBuilder.buildObject(json);//转json

let josn = xmlBuilder.parseString(xml);//转cml
```