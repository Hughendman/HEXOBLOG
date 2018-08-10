---
title: es9
categories: javascript
tags: javascript
keywords: javascript
abbrlink: ea4a72e4
date: 2018-07-10 16:15:00
---

除了es9(es2018)其他只是简单谈谈
## es2016
只有两个小特性：

* includes(),判断一个数组是否包含一个指定的值,返回true或者false

```
let site = ['runoob', 'google', 'taobao'];
 
site.includes('runoob'); 
// true 
 
site.includes('baidu'); 
// false

//案例来自菜鸟教程

```

* a ** b指数运算符，同Math.pow(a,b)


## es2017

* async函数
```
router.get('/',async function (ctx, next) {
  let data;
  await yinxs_a.test_a().then(result => {
    data = result;
  });
  console.log(data);//从数据库中获取的数据
  ctx.body = 'this is a users response!'
})

```
* Object.values ()
```
//es5 Object.keys(obj)

let data = {
    name:'yinx',
    age:'666',
  }
Object.keys(data);

// 返回["name","age"]

//Object.values()
let data = {
    name:'yinx',
    age:'666',
  }
 Object.values(data);

//返回["yinx","666"]


```
* Object.entries()
```

let data = {
    name:'yinx',
    age:'666',
  }
Object.entries(data);
//返回[["name","yinx"],["age","666"]]
```
* Object.getOwnPropertyDescriptors()
```
let data = {
    name:'yinx',
    age:'666',
  }
Object.getOwnPropertyDescriptors(data);

//返回{"name": {"value": "yinx","writable": true,"enumerable": true,"configurable": true},"age": {"value": "666","writable": true,"enumerable": true,"configurable": true}}

```
* padStart()和padEnd()
字符串方法
```
'x'.padStart(5, 'ab')
//返回'ababx'
'x'.padEnd(5, 'ab')
//返回'xabab'

```
* 结尾逗号，数组定义和函数参数列表

ES2017允许函数和数组的最后一个参数有尾逗号

* ShareArrayBuffer和Atomics

共享内存



## es2018

### 异步迭代

首先这段代码是不能执行的
```
async function process(array) {   
    for (let i of array) {     
        await doSomething(i);   
    } 
}

```

es9引入了异步迭代器

```
async function process(array) {   
    for await (let i of array) {    
        doSomething(i);   
    } 
}


```

### Promise.finally()

一个Promise调用链要么成功到达最后一个.then()，要么失败触发.catch()。在某些情况下，你想要在无论Promise运行成功还是失败，运行相同的代码，例如清除，删除对话，关闭数据库连接等。

### Rest/Spread 属性
```
const myObject = {   a: 1,   b: 2,   c: 3 };

const { a, ...x } = myObject; 

// a = 1 // x = { b: 2, c: 3 }

```
### 正则表达式命名捕获组（Regular Expression Named Capture Groups）
ES2018允许命名捕获组使用符号?<name>，在打开捕获括号(后立即命名，示例如下：

```
const   reDate = /(?<year>[0-9]{4})-(?<month>[0-9]{2})-(?<day>[0-9]{2})/,   match  = reDate.exec('2018-04-30'),   year   = match.groups.year,  // 2018   month  = match.groups.month, // 04   day    = match.groups.day;   // 30

```
任何匹配失败的命名组都将返回undefined。


### 正则表达式反向断言（lookbehind）

ES2018引入以相同方式工作但是匹配前面的反向断言（lookbehind），这样我就可以忽略货币符号，单纯的捕获价格的数字：

```
const   reLookbehind = /(?<=\D)\d+/,   match        = reLookbehind.exec('$123.89'); console.log( match[0] ); // 123.89

```

### 正则表达式dotAll模式
正则表达式中点.匹配除回车外的任何单字符，标记s改变这种行为，允许行终止符的出现，例如：

```
/hello.world/.test('hello\nworld');  // false /hello.world/s.test('hello\nworld'); // true

```

### 正则表达式 Unicode 转义

到目前为止，在正则表达式中本地访问 Unicode 字符属性是不被允许的。ES2018添加了 Unicode 属性转义——形式为\p{...}和\P{...}，在正则表达式中使用标记 u (unicode) 设置，在\p块儿内，可以以键值对的方式设置需要匹配的属性而非具体内容。

```
const reGreekSymbol = /\p{Script=Greek}/u; reGreekSymbol.test('π'); // true
```

### 非转义序列的模板字符串
ES2018 移除对 ECMAScript 在带标签的模版字符串中转义序列的语法限制。
### 最后
[参考](http://www.imooc.com/article/37899)

es都出9了，安排（T-T）