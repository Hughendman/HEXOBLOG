---
title: egg.js
categories: node
tags: node
keywords: node
abbrlink: d593ec
date: 2018-06-22 18:15:00
---

[egg.js](http://eggjs.org/zh-cn/intro/quickstart.html)
### 说明

Egg.js 为企业级框架和应用而生，希望由 Egg.js 孕育出更多上层框架，帮助开发团队和开发人员降低开发和维护成本。

### 使用

我们推荐直接使用脚手架，只需几条简单指令，即可快速生成项目:

```
$ npm i egg-init -g
$ egg-init egg-example --type=simple
$ cd egg-example
$ npm i

```

启动项目：

```
$ npm run dev
$ open localhost:7001

```

### 目录结构


* app/router.js 用于配置 URL 路由规则，具体参见 [Router](http://eggjs.org/zh-cn/basics/structure.html)。
* app/controller/** 用于解析用户的输入，处理后返回相应的结果，具体参见 [Controller](http://eggjs.org/zh-cn/basics/controller.html)。
* app/service/** 用于编写业务逻辑层，可选，建议使用，具体参见 [Service](http://eggjs.org/zh-cn/basics/service.html)。
* app/middleware/** 用于编写中间件，可选，具体参见 [Middleware](http://eggjs.org/zh-cn/basics/middleware.html)。
* app/public/** 用于放置静态资源，可选，具体参见内置插件 [egg-static](https://github.com/eggjs/egg-static)。
* app/extend/** 用于框架的扩展，可选，具体参见[框架扩展](http://eggjs.org/zh-cn/basics/extend.html)。
* config/config.{env}.js 用于编写配置文件，具体参见[配置](http://eggjs.org/zh-cn/basics/config.html)。
* config/plugin.js 用于配置需要加载的插件，具体参见[插件](http://eggjs.org/zh-cn/basics/plugin.html)。
* test/** 用于单元测试，具体参见[单元测试](http://eggjs.org/zh-cn/core/unittest.html)。
* app.js 和 agent.js 用于自定义启动时的初始化工作，可选，具体参见[启动自定义](http://eggjs.org/zh-cn/basics/app-start.html)。关于agent.js的作用参见[Agent机制](http://eggjs.org/zh-cn/core/cluster-and-ipc.html#agent-%E6%9C%BA%E5%88%B6)。

由内置插件约定的目录：

* app/public/** 用于放置静态资源，可选，具体参见内置插件 [egg-static](https://github.com/eggjs/egg-static)。
* app/schedule/** 用于定时任务，可选，具体参见[定时任务](http://eggjs.org/zh-cn/basics/schedule.html)。

若需自定义自己的目录规范，参见 [Loader API](https://eggjs.org/zh-cn/advanced/loader.html)

* app/view/** 用于放置模板文件，可选，由模板插件约定，具体参见[模板渲染](http://eggjs.org/zh-cn/core/view.html)。
* app/model/** 用于放置领域模型，可选，由领域类相关插件约定，如 [egg-sequelize](https://github.com/eggjs/egg-sequelize)。
