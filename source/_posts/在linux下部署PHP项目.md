---
title: 在linux下部署PHP项目
categories: PHP
tags: PHP
keywords: PHP
abbrlink: 8197eef2
date: 2018-08-09 16:15:00
---
### 安装httpd

#### 下载

http://httpd.apache.org/download.cgi

我下载的版本为httpd-2.4.34.tar.gz

#### 安装

```

mkdir /usr/local/apache2

tar -zxvf httpd-2.4.34.tar.gz 

cd httpd-2.4.34

./configure --prefix=/usr/local/apache2 --enable-module=shared

```

到这可能会报错

```
checking for APR... no
configure: error: APR not found.  Please read the documentation.

```

解决方案：

```
# APR

mkdir /usr/local/apr

cd  /usr/local/apr

wget http://archive.apache.org/dist/apr/apr-1.5.2.tar.gz

tar -xvzf apr-1.5.2.tar.gz 

cd apr-1.5.2

./configure --with-apr=/usr/local/apr

make

make install


# APR Utils

mkdir /usr/local/apr-util

cd  /usr/local/apr-util

wget http://archive.apache.org/dist/apr/apr-util-1.5.2.tar.gz

tar xvzf apr-util-1.5.2.tar.gz


cd apr-util-1.5.2 

./configure --with-apr=/usr/local/apr --prefix=/usr/local/apr-util

make

make install


```

继续执行

```

cd /usr/local/apache2/httpd-2.4.34

./configure --with-apr-util=/usr/local/apr-util --prefix=/usr/local/apache2 --enable-module=shared

make

make install

```

* 启动Apache：/usr/local/apache2/bin/apachectl start
* 停止Apache：/usr/local/apache2/bin/apachectl stop
* 重启Apache：/usr/local/apache2/bin/apachectl restart


访问 ip:80

### 安装PHP

下载 http://www.php.net/downloads.php

```
mkdir /usr/local/php

cd /usr/local/php/

tar xvzf php-7.1.20.tar.gz

./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache2/bin/apxs  --with-mysql=/var/lib/mysql/
```
可能会报错mysql的

```
./configure --prefix=/usr/local/php --with-apxs2=/usr/local/apache2/bin/apxs --with-config-file-path=/usr/local/lib --enable-track-vars --with-xml --with-mysql-dir=/usr/include/mysql/mysql.h --with-zlib-dir=/usr/lib
```

继续执行：

```
make

make install

cp php.ini-production /usr/local/lib/php.ini 

```

测试：

```
cd /usr/local/apache2/htdocs

tocuh test.php

vim test.php

i

<?php
      phpinfo();
 ?>
 
 访问：ip/test.php

```

### 让apach支持php

```
1.检查apache的配置文件看是否加载了libphp5.so模块，若没有就添加

LoadModule php5_module        modules/libphp5.so

2.在<IfModule mime_module>模块中看是否添加有php页面，若没有就添加

AddType application/x-httpd-php .php .php3 .php4

3.在<IfModule dir_module>模块的DirectoryIndex后添加index.php

```
### 修改默认站点

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
DocumentRoot "/usr/local/apache2/htdocs"
<Directory "/usr/local/apache2/htdocs">


```
