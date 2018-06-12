---
title: 在vue中创建局部组件
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---

## 在main.js中写入

```
import sideBar from './components/public/sideBar.vue';
Vue.component('side-bar', sideBar);

```

> 在使用的时候直接写入<side-bar></side-bar>