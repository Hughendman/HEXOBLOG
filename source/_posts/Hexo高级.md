---
title: Hexo高级使用
categories: 博客
tags: 构建博客
keywords: GitHubPages
abbrlink: 3fb51549
date: 2018-06-12 11:15:00
---

### 添加新的页面

$ hexo new page about


### 配置目录

在next下的_config.yml

```
menu:
  home: / || home
  archives: /archives || archive
  tags: /tags || tags
  message: /message || comment
  about: /about || user

# Enable/Disable menu icons.
menu_icons:
  enable: true

```

### 给一篇文章加入各种属性

```

---
title: 归并排序
categories: 算法
tags: 算法
abbrlink: 14e6f1eb
date: 2018-06-14 11:15:00
---

```

### 本地添加搜索菜单（功能）

```
npm install hexo-generator-searchdb --save

```
打开 站点配置文件 找到Extensions在下面添加

```
search:
     path: search.xml
     field: post
     format: html
     limit: 10000

```


### 添加字数统计、阅读时长、友情链接

* 第一步：安装word_count插件，在博客根目录下打开终端:npm install hexo-wordcount --save
* 第二步：在主题配置文件(themes\next\config.yml)中打开wordcount 统计功能

```
# Post wordcount display settings
    # Dependencies: https://github.com/willin/hexo-wordcount
    post_wordcount:
        item_text: true
        wordcount: true
        min2read: true

```
* 第三步： themes\next\layout_macro\post.swig
将“字”、“分钟” 字样添加到如下位置即可。

```
<span title="{{ __('post.wordcount') }}">
     {{ wordcount(post.content) }} 字
</span>

    ...
<span title="{{ __('post.min2read') }}">
     {{ min2read(post.content) }} 分钟
</span>

```

### 分类和标签

```
在该功能下的index.md中添加

type: "categories"

```

### 首页显示一部分

* 进入themes/next目录下的_config.yml文件
* 搜索"auto_excerpt"：


```
# Automatically Excerpt. Not recommand.
# Please use <!-- more --> in the post to control excerpt accurately.
auto_excerpt:
  enable: false
  length: 150

```

### 添加RSS

```
npm install hexo-generator-feed --save

```

打开 站点配置文件 找到Extensions在下面添加

```

# RSS订阅
feed:
  type: atom
  path: atom.xml
  limit: 20
  hub:
  content:
  content_limit: 140
  content_limit_delim: ' '

```
打开 主题配置文件 找到rss，设置为

```
rss: /atom.xml

```