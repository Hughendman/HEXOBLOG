---
title: element-ui
categories: VUE
tags: VUE
keywords: VUE
abbrlink: d0acb7fa
date: 2018-06-11 11:15:00
---

## 安装
```
cnpm i element-ui --save
```

## 引入

> 在main.js 中

```

import Vue from 'vue'
import ElementUI from 'element-ui'
import 'element-ui/lib/theme-chalk/index.css'
import App from './App.vue'

Vue.use(ElementUI)

new Vue({
  el: '#app',
  render: h => h(App)
})
```

#### [使用菜单导航](https://segmentfault.com/a/1190000007810151)

```

<el-menu :router="true" :default-active="$route.path" class="el-menu-demo" mode="horizontal" @select="handleSelect">
      <el-menu-item index="/table">表格</el-menu-item>
      <el-menu-item index="/form">表单</el-menu-item>
      <el-menu-item index="/graph">图形</el-menu-item>
    </el-menu>
```
> index里面添加路由，  router是使用路由模式为true

> 但是还会发现一个新的问题，它不默认选中了，所以这里要修改一下

```
<el-menu :router="true" :default-active="$route.path" class="el-menu-demo" mode="horizontal" @select="handleSelect">
      <el-menu-item index="/table">表格</el-menu-item>
      <el-menu-item index="/form">表单</el-menu-item>
      <el-menu-item index="/graph">图形</el-menu-item>
    </el-menu>
```
## elementUI关于树状图的增删改查，局部刷新问题

[链接在这里](https://segmentfault.com/a/1190000011574698)


## elementUI关于tree的使用

[getCheckedKeys不能获取父节点的key](https://segmentfault.com/q/1010000012345769/a-1020000012347029)
