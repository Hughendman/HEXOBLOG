---
title: vue的路由（route）
categories: VUE
tags: VUE
keywords: VUE
abbrlink: d41be5a0
date: 2017-06-11 11:15:00
---

## vue的路由

> 注意： component后面的值不要有引号，父级不要有名字name属性

```
[
    {
      path: '/',
      component: Bigdata,
      children: [
        {
          path: '',
          redirect: { name: 'Table' }
        },{
          path: 'table',
          name: 'Table' ,
          component: Table
        }
      ]
    }
  ]

```