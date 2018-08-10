---
title: node读写json文件&Buff（nginx日志中文解决方案）
categories: node
tags: node
keywords: node
abbrlink: bf58f078
date: 2018-07-04 16:15:00
---
# json

## json文件

首先要创建一个json文件，这里推荐一个google插件（WEB前端助手（Fehelper）），可以直接从一个接口中copy一个response，放进插件的输入框中，它会自动格式化代码，并且右上角有一个下载为json格式。但是这个下载的json文件有个问题，需要把第一行的注释去掉。

因为我使用的koa2框架，所以写法我就按照koa2的规则写

## node读文件

```

const fs=require('fs');
const readFile = function(){
    return new Promise(function(resolve,reject){

        fs.readFile( './public/config/staff.json',function (err,data) {
            if(err) return reject(error)
            resolve(JSON.parse(data));
        });
    })
}


let staffJson = await readFile();//这样就可以获取到json里面的内容了

```
## node写文件

```
//query.staff 是你要改写的内容
fs.writeFile('./public/config/staff.json',query.staff,function (err) {
        if(err){
            console.log(err);
        }
    });

```

## try catch 

当使用同步操作时，任何异常都会被立即抛出，可以使用try catch来处理异常

```

const fs = require('fs');

try {
  fs.unlinkSync('/tmp/hello');
  console.log('successfully deleted /tmp/hello');
} catch (err) {
  // handle the error
}

```
# Buffer

Buffer类用来创建一个专门存放二进制数据的缓存区

## 字符编码

```
const buf = Buffer.from('runoob', 'ascii');

// 输出 72756e6f6f62
console.log(buf.toString('hex'));

// 输出 cnVub29i
console.log(buf.toString('base64'));

```

### 重点

前几篇文章中我有写了如何利用nginx进行数据埋点，如果你去实践了，你会发现中文在高版本的nginx中会进行自动编码

让我们来解析一下如何解决这个问题

#### 方案一

降低nginx的版本

#### 调用数据的时候进行解码

根据你的日志将编码后的中文分割，可能是'x','\x','\\\x'。

```
let arr = 'DBxE6x8AxA5xE8xA1xA8'.split('x');
result = [];

```

将 第一个参数进行 Unicode 编码

```
for (let i = 0; i < arr[0].length; i++) {
    result.push(arr[0][i].charCodeAt());
}

```

剩下的参数中前两个字母进行16进制编码，后几个进行Unicode 编码

```
arr.forEach(function(item,index){
    if(index != 0){
        for (let i = 0; i < item.length; i++) {
            if (i == 0 || i == 1) {
                result.push(parseInt(item[0] + item[1], 16));
            } else {
                 result.push(item[i].charCodeAt());
            }
        }
    }
})

```

最后创建一个新的包含字符串 'buffer' 的 UTF-8 字节的 Buffer

```
let buf = Buffer.from(arr);

```

获取中文

```
let zw = buf.toString();

```

### 详解

上面的内容想要理解，我们就需要了解utf-8编码

转自：[字符编码笔记：ASCII，Unicode和UTF-8](https://www.cnblogs.com/ziolo/p/3822454.html)

#### UTF-8

互联网的普及，强烈要求出现一种统一的编码方式。UTF-8就是在互联网上使用最广的一种unicode的实现方式。其他实现方式还包括UTF-16和UTF-32，不过在互联网上基本不用。重复一遍，这里的关系是，UTF-8是Unicode的实现方式之一。

UTF-8最大的一个特点，就是它是一种变长的编码方式。它可以使用1~4个字节表示一个符号，根据不同的符号而变化字节长度。

UTF-8的编码规则很简单，只有二条：

1）对于单字节的符号，字节的第一位设为0，后面7位为这个符号的unicode码。因此对于英语字母，UTF-8编码和ASCII码是相同的(nginx日志中的第一位不是英文就是数字要不就为空)。

2）对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个符号的unicode码（对应第二阶段代码）。