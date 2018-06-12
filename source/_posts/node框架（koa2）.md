---
title: node框架简要说明（express）
date: 2018-06-09 11:15:00
categories : node
tags: 
    - node
    - koa2
keywords : node
---

## 介绍

> 如官网所说要使用koa就要使得node的版本在7以上，但是本人亲测6也是可以的(当然只是一部分，如果要用异步等函数，还是需要升级的)
## node 版本管理
> 如此那么对于不同版本的node，我们有一种方式可以对其进行管理nvm

#### 安装nvm（还有一种n，暂不做介绍，但是nvm只支持mac以及linux）(安了半天，才发现n也不支持window ==)(所以windows下要使用GNVM)
```
安装方式有两种：
$ curl https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh

或者
$ wget -qO- https://raw.github.com/creationix/nvm/v0.4.0/install.sh | sh

```

```
nvm install 0.10  //下载0.10版本node
nvm use 0.10  // 使用指定的版本
nvm ls.nvm  //查看已安装版本
nvm currentv //当前版本
nvm run 0.10.24 server.js  //指定版本运行
rm -rf ~/.nvm 卸载
```
### GNVM(需要使用管理员权限才可以使用)

#### 下载
[32位](https://github.com/Kenshin/gnvm-bin/blob/master/32-bit/gnvm.exe?raw=true)
[64位](https://github.com/Kenshin/gnvm-bin/blob/master/64-bit/gnvm.exe?raw=true)

#### 安装

>  把下载下来的gnvm.exe放到node.exe所在的目录下 
执行gnvm version 检查是否安装成功

#### 命令

```
1、初始化gnvm 
gnvm config INIT

2、查看本地安装了什么版本 
gnvm ls

3、安装需要的版本 
gnvm 4.4.0 6.9.2 7.3.0

4、 gnvm ls查看所有版本后 切换到想要的版本 
gnvm use 6.9.2
```
```
  version                   :: Print the version number of gnvm.exe
  install                   :: Install any node.exe version
  uninstall                 :: Uninstall local node.exe version
  use                       :: Use any version of the local already exists
  update                    :: Update latest node.exe
  ls                        :: List show all <local> <remote> node.exe version
  node-version              :: Show <global> <latest> node.exe version
  config                    :: Setter and getter registry
  help [command]            :: Help about any command
```


## 代码
> 注意koa使用了es6语法，所有需要引入bable（在node大于版本7.6以上的是这么使用）

```
const Koa = require('koa');
const app = new Koa();

const main = ctx => {
  if (ctx.request.accepts('xml')) {
    ctx.response.type = 'xml';
    ctx.response.body = '<data>Hello World</data>';
  } else if (ctx.request.accepts('json')) {
    ctx.response.type = 'json';
    ctx.response.body = { data: 'Hello World' };
  } else if (ctx.request.accepts('html')) {
    ctx.response.type = 'html';
    ctx.response.body = '<p>Hello World</p>';
  } else {
    ctx.response.type = 'text';
    ctx.response.body = 'Hello World';
  }
};

app.use(main);
app.listen(3000);

```
## 详解

#### 假设HTTP服务(和express没有太大区别)

```
const Koa = require('koa');
const app = new Koa();

app.listen(3000);
```

#### Context 对象

> koa提供一个Context 对象，表示一次对话的上下文（res,req）。

```
const Koa = require('koa');
const app = new Koa();

const main = ctx => {  //main函数用来设置ctx.response.body
  ctx.response.body = 'res';
};

app.use(main);//加载main函数
app.listen(3000);
```
> ctx.response是响应   ctx.request是请求

#### response的类型

> ctx.response.type可以判断类型

##### 类型
- xml
- json
- html
- text

#### 文件(同express类似，koa也是fs)

```
const fs = require('fs');

const main = ctx => {
  ctx.response.type = 'html';
  ctx.response.body = fs.createReadStream('./demos/template.html');
};

```

#### 获取href

> ctx.request.path //这个可以用来获取get方式传参过来的参数

#### 路由（koa-route）模块

```
const route = require('koa-route');

const about = ctx => {
  ctx.response.type = 'html';
  ctx.response.body = '<a href="/">Index Page</a>';
};

const main = ctx => {
  ctx.response.body = 'Hello World';
};

app.use(route.get('/', main));//调用到的main函数
app.use(route.get('/about', about));//调用的about函数

```

#### 静态资源(路由)
```
const path = require('path');
const serve = require('koa-static');

const main = serve(path.join(__dirname));
app.use(main);

```

#### 重定向

```
const redirect = ctx => {
  ctx.response.redirect('/');
};

app.use(route.get('/redirect', redirect));

```

#### 日志

```
const main = ctx => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
};

```

#### 中间件

```
const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}
app.use(logger);
```

#### 中间件栈

> 多个中间件会形成一个栈结构（middle stack），以"先进后出"（first-in-last-out）的顺序执行。

- 最外层的中间件首先执行。
- 调用next函数，把执行权交给下一个中间件。
- ...
- 最内层的中间件最后执行。
- 执行结束后，把执行权交回上一层的中间件。
- ...
- 最外层的中间件收回执行权之后，执行next函数后面的代码

```
const one = (ctx, next) => {
  console.log('>> one');
  next();
  console.log('<< one');
}

const two = (ctx, next) => {
  console.log('>> two');
  next(); 
  console.log('<< two');
}

const three = (ctx, next) => {
  console.log('>> three');
  next();
  console.log('<< three');
}

app.use(one);
app.use(two);
app.use(three);

```
> 输出：>> one>> two>> three<< three<< two<< one

#### 异步中间件(async函数)
```
const main = async function (ctx, next) {
  ctx.response.type = 'html';
  ctx.response.body = await fs.readFile('./demos/template.html', 'utf8');
};
```
#### 合成中间件(koa-compose)

```
const compose = require('koa-compose');

const logger = (ctx, next) => {
  console.log(`${Date.now()} ${ctx.request.method} ${ctx.request.url}`);
  next();
}

const main = ctx => {
  ctx.response.body = 'Hello World';
};

const middlewares = compose([logger, main]);//将logger和main合并成一个中间件
app.use(middlewares);
```

#### 500 错误
```
ctx.throw(500)//用来抛出500错误

```
#### 返回状态

```
ctx.response.status = 200;
```

#### 中间件错误处理

```
const handler = async (ctx, next) => {
  try {
    await next();
  } catch (err) {
    ctx.response.status = err.statusCode || err.status || 500;
    ctx.response.body = {
      message: err.message
    };
  }
};

const main = ctx => {
  ctx.throw(500);
};

app.use(handler);
app.use(main);
```
#### 运行出错会监听一个error事件
```
app.on('error', (err, ctx) =>
  console.error('server error', err);
);
```
> 使用try catch 是不能使用上面的方法的，这时候使用的是
ctx.app.emit('error', err, ctx);手动释放error

#### 使用cookie
```
const n = Number(ctx.cookies.get('view') || 0) + 1;
ctx.cookies.set('view', n);
```


#### 获取post的请求参数

```
const koaBody = require('koa-body');

const main = async function(ctx) {
  const body = ctx.request.body;
  if (!body.name) ctx.throw(400, '.name required');
  ctx.body = { name: body.name };
};

app.use(koaBody());

```
#### 利用koa-body进行文件上传
```
const os = require('os');
const path = require('path');
const koaBody = require('koa-body');

const main = async function(ctx) {
  const tmpdir = os.tmpdir();
  const filePaths = [];
  const files = ctx.request.body.files || {};

  for (let key in files) {
    const file = files[key];
    const filePath = path.join(tmpdir, file.name);
    const reader = fs.createReadStream(file.path);
    const writer = fs.createWriteStream(filePath);
    reader.pipe(writer);
    filePaths.push(filePath);
  }

  ctx.body = filePaths;
};

app.use(koaBody({ multipart: true }));
```

#### 参考文献


[阮一峰教程](http://www.ruanyifeng.com/blog/2017/08/koa.html)

[koa workshop](https://koa.bootcss.com/)

[kick-off-koa](https://github.com/koajs/kick-off-koa)

[Koa Examples](https://github.com/koajs/examples)




































