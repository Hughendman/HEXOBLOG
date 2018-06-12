---
title: Hexo高级使用
date: 2018-06-12 11:15:00
categories : 博客
tags: 构建博客
keywords : GitHubPages
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