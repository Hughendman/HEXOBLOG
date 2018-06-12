---
title: vue中使用sass
date: 2018-06-11 11:15:00
categories : VUE
tags: VUE
keywords : VUE
---

### 安装

```
npm install node-sass --save-dev
npm install sass-loader --save-dev

```

## 配置

> 打开webpack.base.config.js在loaders里面加上  module -- rules

```
rules: [
      {
        test: /\.vue$/,
        loader: 'vue-loader',
        options: vueLoaderConfig
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src'), resolve('test')]
      },
      {
        test: /\.(png|jpe?g|gif|svg)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('img/[name].[hash:7].[ext]')
        }
      },
      {
        test: /\.scss$/,
        loaders: ["style", "css", "sass"]
      },//这里这里看这里，主要引入这一段
      {
        test: /\.(woff2?|eot|ttf|otf)(\?.*)?$/,
        loader: 'url-loader',
        query: {
          limit: 10000,
          name: utils.assetsPath('fonts/[name].[hash:7].[ext]')
        }
      }
    ]
  }

```

