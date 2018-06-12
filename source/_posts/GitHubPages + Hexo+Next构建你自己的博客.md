---
title: GitHubPages + Hexo+Next构建你自己的博客
date: 2018-06-11 11:15:00
categories : 博客
tags: 构建博客
keywords : GitHubPages
---

### github准备

新建一个项目：你的用户名.github.io（项目名称）

### Hexo

* 安装node
* npm install hexo-cli -g
* hexo init 你的用户名.github.io // 尽量和Github仓库使用同一个名称
* cd 你的用户名.github.io
* hexo s
* 打开http://localhost:4000/
* 

### Next主题安装

* Next主题是iissnan所创作的一个Hexo主题，以简洁为主
* cd xxx.github.io 
* git clone https://github.com/iissnan/hexo-theme-next themes/next

##### _config.yml站点配置

```
  # Hexo Configuration
      ## Docs: https://hexo.io/docs/configuration.html
      ## Source: https://github.com/hexojs/hexo/

      # Site
      title: 最晚的开始   #站点名称
      subtitle: 所有的为时已晚都是开始的最好时候  #副标题
      #个人描述
      description: My goal is not write code.if we could ship products and make all this money without writing any code,we could.Your job is ship products EXACTLY on time.It doesn’t matter whether you are a developer,tester,program manager,product manager whatever.Everybody’s job is the same. 
      author: Jack_lin  #作者
      language: zh-Hans  #语言
      timezone:

      # URL   
      #绑定域名后，要创建 sitemap.xml 时再配置该项 
      ## If your site is put in a subdirectory, set url as   'http://yoursite.com/child' and root as '/child/'
      url: http://yoursite.com
      root: /
      permalink: :year/:month/:day/:title/
      permalink_defaults:

      # Directory   #目录不用修改
      source_dir: source
      public_dir: public
      tag_dir: tags
      archive_dir: archives
      category_dir: categories
      code_dir: downloads/code
      i18n_dir: :lang
      skip_render:

      # Writing
      # 文章布局、写作格式的定义，不修改
      new_post_name: :title.md # File name of new posts
      default_layout: post
      titlecase: false # Transform title into titlecase
      external_link: true # Open external links in new tab
      filename_case: 0
      render_drafts: false
      post_asset_folder: false
      relative_link: false
      future: true
      highlight:
      enable: true
      line_number: true
      auto_detect: false
      tab_replace:

      # Category & Tag
      default_category: uncategorized
      category_map:
      tag_map:

      # Date / Time format  #时间格式不用修改
      ## Hexo uses Moment.js to parse and display date
      ## You can customize the date format as defined in
      ## http://momentjs.com/docs/#/displaying/format/
      date_format: YYYY-MM-DD
      time_format: HH:mm:ss

      # Pagination   #每页显示文章数，可以自行定义
      ## Set per_page to 0 to disable pagination
      per_page: 10
      pagination_dir: page

      # Extensions
      #配置站点时，所使用的主题和插件，切换主题可以在这里设置
      ## Plugins: https://hexo.io/plugins/
      ## Themes: https://hexo.io/themes/
      theme: next //在这里切换主题
       # theme: landscape

      # 头像， 在xxx.github.io/source 下相对路径，若source文件夹下没有uploads，就新建一个名为uploads文件夹，具体见下面截图
      avatar: /uploads/images/avatar.png

      # Deployment
      #这里是部署到Github上的设置
      ## Docs: https://hexo.io/docs/deployment.html
      deploy:
      type: git  #git提交
      repo: https://github.com/123sunxiaolin/123sunxiaolin.github.io.git  #已创建的Github仓库
      branch: master #提交到的分支

```


如果报错: ERROR Deployer not found: git
> npm install --save hexo-deployer-git

### 部署

> hexo clean && hexo g && hexo d

打包的项目在public中





