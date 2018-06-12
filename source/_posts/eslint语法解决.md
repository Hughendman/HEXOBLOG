---
title: eslint语法解决
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---

```
在webpack.base.conf.js里面删掉下面:

preLoaders: [
      {
        test: /\.vue$/,
        loader: 'eslint',
        include: projectRoot,
        exclude: [/node_modules/, /ignore_lib/]
      },
      {
        test: /\.js$/,
        loader: 'eslint',
        include: projectRoot,
        exclude: [/node_modules/, /ignore_lib/]
      }
    ]
```

```

删除以下代码就可以
{

    test: /\.(js|vue)$/,
    loader: 'eslint-loader',
    enforce: 'pre',
    include: [resolve('src'), resolve('test')],
    options: {
      formatter: require('eslint-friendly-formatter')
    }
```

> 然后需要重新编辑生效