---
title: Vuex（关于vue的状态管理模式）
categories: VUE
tags: VUE
keywords: VUE
abbrlink: 2fe08ed3
date: 2017-06-11 11:15:00
---

## 介绍vuex

> vuex是一个为了vue开发的状态管理模式，它采用集中式存储管理应用的所有组件的状态，并以相应的规则保证状态以一种可预测的方式发生变化，

[这篇文章不错](http://blog.csdn.net/github_26672553/article/details/53176778)


### 状态管理模式

```
new Vue({
  // state
  data () {
    return {
      count: 0
    }
  },
  // view
  template: `
    <div>{{ count }}</div>
  `,
  // actions
  methods: {
    increment () {
      this.count++
    }
  }
})

```

> 如上所示 state为驱动应用的数据源，view以声明的方式将state映射到视图，actions为响应在view上的用户输入导致的状态变化

> 单向数据流

```
graph LR;  
　　Actions-->State;   
　　State-->View;  
　　View-->Actions;
　　
```

> 如上所示为单向数据流，但是当我们有多个组件共享状态时，单向数据流的简洁性很容易被破坏。

> 这时会有两个需求多个视图依赖于同一状态，来自不同视图的行为需要变更同一状态。

> 为了方便我们把组件的共享状态抽取出来，以一个全局单例模式管理（Vuex）

## 安装

```
npm i vuex --save
```

### 引入

```
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)
```

## 使用

