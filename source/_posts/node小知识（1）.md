---
title: node小知识（1）
categories: node
tags: node
keywords: node
abbrlink: da8c6d93
date: 2018-06-21 11:15:00
---

### Nodejs优缺点

#### 优点

* 事件驱动，通过闭包很容易实现客户端的生命活期。
* 不用担心多线程，锁，并行计算的问题
* V8引擎速度非常快
* 对于游戏来说，写一遍游戏逻辑代码，前端后端通用


#### 缺点

* nodejs更新很快，可能会出现版本兼容
* nodejs还不算成熟，还没有大制作
* nodejs不像其他的服务器，对于不同的链接，不支持进程和线程操作

### 预防[timing attacks](https://en.wikipedia.org/wiki/Timing_attack)攻击

```
function checkApiKey(apiKeyFromDb, apiKeyReceived)
{
    if (apiKeyFromDb === apiKeyReceived)
    {
        return true
    }
    return false
}

```
比较密码时，不能泄露任何信息，因此比较必须在固定时间完成。否则，可以使用timing attacks来攻击你的应用。为什么会这样呢?Node.js使用V8引擎，它会从性能角度优化代码。它会逐个比较字符串的字母，一旦发现不匹配时就停止比较。当攻击者的密码更准确时，比较的时间越长。因此，攻击者可以通过比较的时间长短来判断密码的正确性。使用[cryptiles](https://www.npmjs.com/package/cryptiles)可以解决这个问题:

```
function checkApiKey(apiKeyFromDb, apiKeyReceived)
{
    return cryptiles.fixedTimeComparison(apiKeyFromDb, apiKeyReceived)
}

```


### 什么是错误优先的回调函数？

错误优先的回调函数(Error-First Callback)用于同时返回错误和数据。第一个参数返回错误，并且验证它是否出错；其他参数用于返回数据。

```
fs.readFile(filePath, function(err, data)
{
    if (err)
    {
        // 处理错误
        return err;
    }
    console.log(data);
});

```

### 如何避免回调地狱？

* 模块化: 将回调函数转换为独立的函数
* 使用流程控制库，例如[aync](https://www.npmjs.com/package/async)
* 使用Promise
* 使用aync/await([参考Async/Await替代Promise的6个理由](https://blog.fundebug.com/2017/04/04/nodejs-async-await/))


### 用什么工具保证一致的代码风格？为什么要这样？

团队协作时，保证一致的代码风格是非常重要的，这样团队成员才可以更快地修改代码，而不需要每次去适应新的风格。这些工具可以帮助我们：


* [ESLint](http://eslint.org/)
* [Standard](https://standardjs.com/)
* JSLint
* JSHint
* ESLint
* JSCS推荐


### 什么是事件循环

* Node采用的是单线程的处理机制(所有的I/O请求都采用非阻塞的工作方式)，至少从Node.js开发者的角度是这样的。而在底层，Node.js借助libuv来作为抽象封装层，从而屏蔽不同操作系统的差异，Node可以借助libuv来实现线程。
* Libuv库负责Node API的执行。它将不同的任务分配给不同的线程，形成一个事件循环，以异步的方式将任务的执行结果返回给V8引擎。
* 每一个I/O都需要一个回调函数————一旦执行完便堆到事件循环上用于执行

### Cookies如何防范XSS攻击？

SS(Cross-Site Scripting，跨站脚本攻击)是指攻击者在返回的HTML中插入JavaScript脚本。为了减轻这些攻击，需要在HTTP头部配置set-cookie:

* HttpOnly - 这个属性可以防止cross-site scripting，因为它会禁止Javascript脚本访问cookie。
* secure - 这个属性告诉浏览器仅在请求为HTTPS时发送cookie

结果应该是这样的: Set-Cookie: sid=<cookie-value>; HttpOnly. 

### 使用监控unhandledRejection事件来捕获所有未处理的Promise错误:

```
process.on('unhandledRejection', (err) =>
{
    console.log(err)
})

```

### 什么是Promise?

Promise是一个构造函数，有属于自己私有的all,reject,resolve,rece等方法，也有原型上面的，属于实例对象调用的方法then,catch

```
// Promise里面传入一个函数类型的参数，这个函数类型的参数接收两个参数resolve reject
var p=new Promise(function(resolve,reject){
     // 异步操作
     setTimeout(function(){
         console.log('icessun');  // 两秒之后打印出icessun
         resolve('icessun2'); // resolve是成功后的回调函数 里面的icessun2是传入的参数
      },2000)
  });
// 那么p是一个实例对象，可以使用then方法（Promise原型上面的方法）
p.then(function(){
    console.log(arguments);  // 会打印出一个类数组 ['icessun2']
    
 })
p.then(function(data){
    console.log(data);  // 会打印出icessun2 data接收了resolve里面的参数
 })

```
于上面这段代码，首先new一个实例对象赋值给p，Promise的构造函数接受一个参数，是函数；并且传入两个参数：resolve,reject，分别表示异步操作执行成功后的回调函数和异步操作执行失败后的回调函数；然后里面设置一个定时器setTimeout，开启一个异步操作，两秒后输出icessun，并且调用resolve方法

```

function icessun(){
   var p=new Promise(function(resolve,reject){
        setTimeout(function(){
           console.log('icessun');
           reslove('icessun2');
        },2000);
    });
  return p; // 返回p实例，使其可以使用Promise原型上面的方法
}
icessun(); // 调用执行icessun函数 得到一个Promis对象

// 也可以直接这样调用
icessun().then(function(data){
console.log(data); // icessun2
// 一些其他的操作
// .....
});

作者：icessun
链接：https://www.jianshu.com/p/43f948051d65
來源：简书
简书著作权归作者所有，任何形式的转载都请联系作者获得授权并注明出处。
```



### promsie 

```
Promise.resolve(1)  
  .then((x) => x + 1)
  .then((x) => { throw new Error('My Error') })
  .catch(() => 1)
  .then((x) => x + 1)
  .then((x) => console.log(x))
  .catch(console.error)

```
答案是2，逐行解释如下:

1. 创建新的Promise，resolve值为1。
2. x为1，加1之后返回2。
3. x为2，但是没有用到。抛出一个错误。
4. 捕获错误，但是没有处理。返回1。
5. x为1，加1之后返回2。
6. x为2，打印2。
7. 不会执行，因为没有错误抛出。



### 参考

[Nodejs2017](https://cnodejs.org/topic/58eb64293145ae3f25fe614c)

[Promise用法](https://www.jianshu.com/p/43f948051d65)



