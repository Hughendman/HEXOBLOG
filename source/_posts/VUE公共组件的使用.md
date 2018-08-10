---
title: VUE公共组件的使用
categories: VUE
tags: VUE
keywords: VUE
abbrlink: 8d07ac1f
date: 2018-06-20 16:15:00
---

## 创建一个组件

imgTarg.vue

```
<template>
......
</template>
<script>

    export default {
        props:["imgUrl","keyId"]
    }
    
</script>
<style scoped>
</style>

```

## 引入组件

```
import imgTarg from './components/imgTarg'
Vue.component('img-targ', imgTarg);

```

## 使用(在需要的组件中)

```
<img-targ v-for="(data,index) in data" :key="index" :imgUrl="data" :keyId="index"></img-targ>

```