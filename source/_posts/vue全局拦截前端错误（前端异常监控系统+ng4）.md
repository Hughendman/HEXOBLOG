---
title: vue全局拦截前端错误（前端异常监控系统+ng4）
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---

## errorHandler

> 一般来讲，前端的异常处理使用的是try catch 和window.onerror 但是在框架中就不可以了，三大前端框架只有react可以这么使用，vue中有自己的errorHandler，ng也有自己的属性可以这么用

> 这里面我们用errorHandler来进行错误拦截

```
Vue.config.errorHandler = function (err, vm, info) {
  // handle error
  // `info` 是 Vue 特定的错误信息，比如错误所在的生命周期钩子
  // 只在 2.2.0+ 可用
}

```
```
//具体报错信息

ReferenceError: i is not defined  //err
    at VueComponent.created (bigdata.vue?1536:26)
    at callHook (vue.esm.js?5425:2895)
    at VueComponent.Vue._init (vue.esm.js?5425:4560)
    at new VueComponent (vue.esm.js?5425:4728)
    at createComponentInstanceForVnode (vue.esm.js?5425:4242)
    at init (vue.esm.js?5425:4059)
    at createComponent (vue.esm.js?5425:5512)
    at createElm (vue.esm.js?5425:5460)
    at createChildren (vue.esm.js?5425:5586)
    at createElm (vue.esm.js?5425:5488)

VueComponent {_uid: 4, _isVue: true, $options: {…},  //vm _renderProxy: Proxy, _self: VueComponent, …}


created hook  //info


```
[vue2.0参考这里](https://cn.vuejs.org/v2/api/#errorHandler )

[ng4参考这里](https://www.angular.cn/api/core/ErrorHandler)

## 接口形式同埋点上传（利用一个gif的形式传递数据）

[这是我以前写的关于如何进行埋点上传的文章(代码不是我写的)](http://note.youdao.com/noteshare?id=f3283d431c12eaae4fe962df2b486755&sub=9D6EE15F28F74A6CAFFE096C2B5178BE)