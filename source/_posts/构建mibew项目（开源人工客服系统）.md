---
title: 构建mibew项目（开源人工客服系统）
categories: PHP
tags: PHP
keywords: PHP
abbrlink: 60c9682d
date: 2018-08-13 16:15:00
---

### 项目环境

* php版本为5，千万别用7

* apache

* mysql

具体安装可以参考我的博客
[在linux下部署PHP项目](https://hughendman.github.io/post/8197eef2.html#more)

### git地址
我在自己的git上配置过一个版本3的代码，可以借鉴。如果不喜欢请自己寻找其他版本的mibew

[git下载地址](https://github.com/Hughendman/mibew)


[代码源码地址](https://sourceforge.net/projects/mibew/files/core/3.1.3/mibew-3.1.3.zip/download)
```
git clone git@github.com:Hughendman/mibew.git

```

### 部署

修改apache文件

```
/usr/local/apache2/conf

vim httpd.conf

```

我将项目下载到了/var/www下面

```
<Directory />
    AllowOverride none
    Require all denied
</Directory>

#
# Note that from this point forward you must specifically allow
# particular features to be enabled - so if something's not working as
# you might expect, make sure that you have specifically enabled it
# below.
#

#
# DocumentRoot: The directory out of which you will serve your
# documents. By default, all requests are taken from this directory, but
# symbolic links and aliases may be used to point to other locations.
#
DocumentRoot "/var/www"
<Directory "/var/www">


```
修改数据库配置

```
cd /var/www/mibew/configs

vim config.yml

```

```
database:
    host: "localhost"
    port: 3306
    db: "mibew"
    login: "root"
    pass: "abcd234"
    tables_prefix: ""
    use_persistent_connection: false

```

重启apache

```
/usr/local/apache/bin/apachectl restart

```

### 应用

访问：http://127.0.0.1/mibew/install.php

下载数据库


访问：http://127.0.0.1/mibew/index.php/operator/login


用户：admin 密码：admin

### 嵌入其他应用

进入：http://127.0.0.1/mibew/index.php/operator/button-code

可以获取嵌入代码。

例如：

```
<!-- mibew button --><a id="mibew-agent-button" href="/mibew/index.php/chat?locale=en" target="_blank" onclick="Mibew.Objects.ChatPopups['5b7126f35626d57a'].open();return false;"><img src="/mibew/index.php/b?i=mibew&amp;lang=en" border="0" alt="" /></a><script type="text/javascript" src="/mibew/js/compiled/chat_popup.js"></script><script type="text/javascript">Mibew.ChatPopup.init({"id":"5b7126f35626d57a","url":"\/mibew\/index.php\/chat?locale=en","preferIFrame":true,"modSecurity":false,"forceSecure":false,"width":640,"height":480,"resizable":true,"styleLoader":"\/mibew\/index.php\/chat\/style\/popup"});</script><!-- / mibew button -->

```

修改一下：

```

<!-- mibew button --><a id="mibew-agent-button" href="http://127.0.0.1/mibew/index.php/chat?locale=en" target="_blank" onclick="Mibew.Objects.ChatPopups['5b7126f35626d57a'].open();return false;"><img src="http://127.0.0.1/mibew/index.php/b?i=mibew&amp;lang=en" border="0" alt="" /></a><script type="text/javascript" src="http://127.0.0.1/mibew/js/compiled/chat_popup.js"></script><script type="text/javascript">Mibew.ChatPopup.init({"id":"5b7126f35626d57a","url":"http://127.0.0.1\/mibew\/index.php\/chat?locale=en","preferIFrame":true,"modSecurity":false,"forceSecure":false,"width":640,"height":480,"resizable":true,"styleLoader":"http://127.0.0.1\/mibew\/index.php\/chat\/style\/popup"});</script><!-- / mibew button -->

```

这样就可以使用了。

进入 http://127.0.0.1/mibew/index.php/operator/users

可以操作了

一下为实例：

如果你嵌入代码成功，你会在你的代码中发现这样一个按钮

![](https://raw.githubusercontent.com/Hughendman/picture/master/mibew/1.png)

点击这个按钮，会在页面右侧弹出

![](https://raw.githubusercontent.com/Hughendman/picture/master/mibew/2.png)


下面介绍以下管理员页面：

![](https://raw.githubusercontent.com/Hughendman/picture/master/mibew/3.png)

![](https://raw.githubusercontent.com/Hughendman/picture/master/mibew/4.png)

![](https://raw.githubusercontent.com/Hughendman/picture/master/mibew/5.png)