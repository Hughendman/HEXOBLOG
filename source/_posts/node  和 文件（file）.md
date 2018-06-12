
---
title: node  和 文件（file）
date: 2018-06-09 11:15:00
categories : node
tags: node
keywords : node
---

## fs
#### 文件读取


> 实现方法
首先创建一个file.txt的文件

```
var fs = require('fs');  
  
fs.readFile('file.txt', 'utf-8', function (err, data) {  
    if (err) {  
        console.log("读取错误：" + err);  
        return;  
    }  
    console.log(data);  
})  

```

> 注意这里需要指定第二个参数，否则的话会默认输出十六进制字节的格式

> 注意如果文件编码为ANSI格式，还用‘utf-8’会读取出错

> 还用这些格式utf8, ucs2,ascii, binary, base64, hex可以供我们使用

#### 同步读取文件

```
var fs = require('fs');  
var data = fs.readFileSync('test.txt', 'utf-8');  
console.log(data);

```
[具体的 操作可以看这里](http://blog.csdn.net/qq20004604/article/details/51646689)
