---
title: 快速构建vue项目（vue-cli）
categories: VUE
tags: VUE
keywords: VUE
abbrlink: fb88d540
date: 2018-06-11 11:15:00
---

## 全局安装
```
npm i -g vue-cli
```

## 创建项目(项目名称叫muses)

```
vue init webpack muses
```

## 启动(默认的是80端)

```
npm run dev
```

## 详细讲解

> 与ng4 的cli 不同 ，vue的cli比较复杂 ，但是实际上只要一直回车对后面没有影响，注意最后一项要选择npm的那个选项

> build文件里面是一些操作文件,执行 npm run * 时执行的就是这里的文件

> config 文件是配置文件

> src 是资源文件，组件等都放在这里

> assets 资源文件，同ng4 类似，放的是公共的资源，例如图片

> 

## 打包

```
npm run build
```

## 注意

> 如果你是用的编辑器是webstorm 那么需要在setting中的Language选项里面的JavaScript设置为ECMAScript 6，这样才可以使用，如果还有问题，那么需要在script标签中标注type为es6

