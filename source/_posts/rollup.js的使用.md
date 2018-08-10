---
title: rollup.js的使用
categories: 打包工具
tags: 打包工具
keywords: 打包工具
abbrlink: d0055c9e
date: 2018-06-21 16:15:00
---

### 全局安装rollup


```
npm install -g rollup

```

### 构建外部文件(src/basic.js)


```
export function u() {
    try{
        return 'uid';
    }catch (e){
        console.log(e);
    }
}

```

### 构建入口文件(src/main.js)

```
import { u } from './basic';

console.log(u());


```


### 构建打包文件(rollup.config.js)


```
export default {
    entry: 'src/main.js', //入口文件
    format: 'cjs',
    dest: 'rel/bundle.js' // 输出文件
};

```

### 执行打包命令

```
rollup -c

```


### 附件

[API讲解](https://www.cnblogs.com/tugenhua0707/p/8179686.html)

[实例](https://github.com/tugenhua0707/rollup-demo)