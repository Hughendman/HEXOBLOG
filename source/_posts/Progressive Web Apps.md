---
title: Progressive Web Apps
categories: PWA
tags: PWA
keywords: PWA
abbrlink: a9b40ecc
date: 2018-06-13 11:15:00
---

> 今天想把muses项目升级为pwa，在本地运行还可以，但布到线上的时候，发现一直在报一个错误（Only secure origins are allowed (see: https://goo.gl/Y0ZkNV)），解决办法是将http协议升级为https，可以用nginx实现，但是需要证书

> 这是我们可以使用node创建一个， [这是如何利用node启动https](http://blog.fens.me/nodejs-https-server/)


> 下面是app.js的代码

```
const express = require('express')
const path = require('path')
const app = express()

app.use(express.static(path.join(__dirname, 'public')))

var https = require('https')
    ,fs = require("fs");

var options = {
    key: fs.readFileSync('./privatekey.pem'),
    cert: fs.readFileSync('./certificate.pem')
};

https.createServer(options, app).listen(3011, function () {
    console.log('Https server listening on port ' + 3011);
});
```

## 介绍

> Progressive Web Apps 是 Google 提出的用前沿的 Web 技术为网页提供 App 般使用体验的一系列方案。这篇文章里我们来完成一个非常简单的 PWA 页面。

> 一个 PWA 应用首先是一个网页, 可以通过 Web 技术编写出一个网页应用. 随后添加上 App Manifest 和 Service Worker 来实现 PWA 的安装和离线等功能

[这是一个案例](https://github.com/minimal-xyz/minimal-pwa)


[这是一个比较复杂的案例](https://github.com/sitepoint-editors/pwa-retrofit.git)
> 使用方法，http-server启动，打开80端口，kill掉服务，再刷新，发现仍然存在

## service worker生命周期

![service worker生命周期](http://jbcdn2.b0.upaiyun.com/2016/01/55b0169cdfe92b08203757ebc4e5ece2.png)

[参考](http://web.jobbole.com/84792/)

## 安装

> 需要安装http-server和ngrok以便调试和查看

> 准备一个HTML文件

## 添加manifest.json文件

> 使用manifest.json定义应用的名称，图标等等信息。

```
{
  "name": "Minimal app to try PWA",
  "short_name": "Minimal PWA",
  "display": "standalone",
  "start_url": "/",
  "theme_color": "#8888ff",
  "background_color": "#aaaaff",
  "icons": [
    {
      "src": "e.png",
      "sizes": "256x256",
      "type": "image/png"
    }
  ]
}
```


> 然后引入到html文件之中

```
<link rel="manifest" href="manifest.json" />
```


## 添加Service Worker

> Service Worker 在网页已经关闭的情况下还可以运行, 用来实现页面的缓存和离线, 后台通知等等功能。sw.js 文件需要在 HTML 当中引入:

```
<script>
  if (navigator.serviceWorker != null) {
    navigator.serviceWorker.register('sw.js')
    .then(function(registration) {
      console.log('Registered events at scope: ', registration.scope);
    });
  }
</script>
```

#### 后面我们会往 sw.js 文件当中添加逻辑代码。在 Service Worker 当中会用到一些全局变量:
- self: 表示 Service Worker 作用域, 也是全局变量
- caches: 表示缓存
- skipWaiting: 表示强制当前处在 waiting 状态的脚本进入 activate 状态
- clients: 表示 Service Worker 接管的页面


## 处理静态缓存

#### 首先定义需要缓存的路径, 以及需要缓存的静态文件的列表, 这个列表也可以通过 Webpack 插件生成。

```
var cacheStorageKey = 'minimal-pwa-1'

var cacheList = [
  '/',
  "index.html",
  "main.css",
  "e.png"
]
```

#### 借助 Service Worker, 可以在注册完成安装 Service Worker 时, 抓取资源写入缓存:

```
self.addEventListener('install', e => {
  e.waitUntil(
    caches.open(cacheStorageKey)
    .then(cache => cache.addAll(cacheList))
    .then(() => self.skipWaiting())
  )
})

```

#### 调用 self.skipWaiting() 方法是为了在页面更新的过程当中, 新的 Service Worker 脚本能立即激活和生效。

## 处理动态缓存

#### 网页抓取资源的过程中, 在 Service Worker 可以捕获到 fetch 事件, 可以编写代码决定如何响应资源的请求:

```
self.addEventListener('fetch', function(e) {
  e.respondWith(
    caches.match(e.request).then(function(response) {
      if (response != null) {
        return response
      }
      return fetch(e.request.url)
    })
  )
})

```
#### 真实的项目当中, 可以根据资源的类型, 站点的特点, 可以专门设计复杂的策略。fetch 事件当中甚至可以手动生成 Response 返回给页面。


## 更新静态资源

#### 缓存的资源随着版本的更新会过期, 所以会根据缓存的字符串名称(这里变量为 cacheStorageKey, 值用了 "minimal-pwa-1")清除旧缓存, 可以遍历所有的缓存名称逐一判断决决定是否清除(备注: 简化的写法, Promise.all 中 return undefined 可能出错):

```

self.addEventListener('activate', function(e) {
  e.waitUntil(
    Promise.all(
      caches.keys().then(cacheNames => {
        return cacheNames.map(name => {
          if (name !== cacheStorageKey) {
            return caches.delete(name)
          }
        })
      })
    ).then(() => {
      return self.clients.claim()
    })
  )
})
```

#### 在新安装的 Service Worker 中通过调用 self.clients.claim() 取得页面的控制权, 这样之后打开页面都会使用版本更新的缓存。旧的 Service Worker 脚本不再控制着页面之后会被停止


## 查看 Demo

#### 执行命令

> http-server -c-1 # 注意设置关闭缓存, 这里用参数 -c-1   
> #用另一个终端
ngrok http 8080

> 桌面浏览器可以直接通过 http://localhost:8080 访问, 从 DevTools 的 Application 标签可以看到 Service Worker。

> 由于 Service Worker 限制了使用 HTTPS 地址或者 localhost 地址, 在 Android Chrome 打开需要借助 ngrok 生成的 HTTPS 地址, 这样才能把 demo 添加到首屏。添加到首屏之后, 即便在离线状态下, 页面也可以打开。




