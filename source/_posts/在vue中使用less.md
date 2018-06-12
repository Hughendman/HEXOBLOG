---
title: 在vue中使用less
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---


## 安装

> cnpm install less less-loader --save

> 在webpack.base.config.js在loaders里面加上


```

{

test: /\.less$/,

loader: "style-loader!css-loader!less-loader",

},
```


<style scoped lang="less">