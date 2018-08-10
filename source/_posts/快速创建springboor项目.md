---
title: 快速创建springboot项目(1)
categories: SpringBoot
tags: SpringBoot
keywords: SpringBoot
abbrlink: f7ca40ba
date: 2018-07-24 16:15:00
---

## idea
[参考慕课网](https://www.imooc.com/video/13590)
### 构建项目

![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/1.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/2.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/3.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/4.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/5.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/6.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/7.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/8.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/9.png)


### 运行

![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/10.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/11.png)

打开80端口
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/12.png)

## eclipse

### 构建项目
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/13.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/14.png)

![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/17.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/18.png)
最后点击finish

### 运行
![](https://raw.githubusercontent.com/Hughendman/picture/master/springboot/19.png)

### 错误处理

在eclipse运行时失败，要进入项目目录执行mvn clean install，再回到eclipse重新执行即可

如果你打开之后发现没有运行tomcat，修改spring boot的版本号。（目前我使用的是1.4.7）