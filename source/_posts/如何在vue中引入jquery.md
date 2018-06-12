---
title: 如何在vue中引入jquery
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---

## 安装

> 在package.json里的dependencies加入"jquery" : "^2.2.3"，然后install 或者直接安装也可以

```
在webpack.base.conf.js里加入

var webpack = require("webpack")

在module.exports的最后加入

plugins: [
new webpack.ProvidePlugin({
jQuery: "jquery",
$: "jquery"
})
]

然后一定要重新 run dev

在main.js 引入就ok了import $ from 'jquery'

```