---
title: 报错信息
categories: JAVA
tags: JAVA
keywords: JAVA
abbrlink: 624671ed
date: 2018-06-19 16:15:00
---
# [报错项目](https://github.com/xianrendzw/EasyReport/blob/master/docs/manual/version2_0.md)

### 1

```
No compiler is provided in this environment. Perhaps you are running on a JRE rather than a JDK?

```

原因：环境变量配置错误，配置到了jdk下面


### 2

```
Failed to execute goal org.apache.maven.plugins:maven-surefire-plugin:2.18.1:test (default-test) 

```

解决方案：

```
mvn clean install -DskipTests

```

### 3

```
 Could not resolve dependencies for project com.easytoolsoft:easyreport-engine:jar:2.1-SNAPSHOT: The following artifacts could not be resolved: com.microsoft.sqlserver:sqljdbc4:jar:4.0, com.oracle:ojdbc6:jar:11.2.0.3: Failure to find com.microsoft.sqlserver:sqljdbc4:jar:4.0 in https://repo.maven.apache.org/maven2 was cached in the local repository, resolution will not be reattempted until the update interval of central has elapsed or updates are forced
 
 ```
 
 原因： 微软不允许以maven的方式直接下载该文件
 
 解决办法：下载[sqljdbc4.jar](https://pan.baidu.com/s/1wpYSMWrkihb7ZQpHXs-RXw)(密码：hrt0)改名为sqljdbc4-4.0.jar，放到maven库里
 
 ```
 mvn help:effective-settings //查找本地maven库
 
 ```
 
 ```
 C:\Users\admin\.m2\repository //我的库的地址（默认）
 
 ```
 根据上面找到下面的目录，将sqljdbc4-4.0.jar放入
 ```
 C:\Users\admin\.m2\repository\com\microsoft\sqlserver\sqljdbc4\4.0
 
 ```
 
### 4

```
Could not resolve dependencies for project com.easytoolsoft:easyreport-engine:jar:2.1-SNAPSHOT: Failure to find com.oracle:ojdbc6:jar:11.2.0.3 in https://repo.maven.apache.org/maven2 was cached in the local repository, resolution will not be reattempted until the update interval of central has elapsed or updates are forced 

```

原因：Oracle的ojdbc.jar是收费的，所以maven的中央仓库中没有这个资源，只能通过配置本地库才能加载到项目中去。

下载
http://central.maven.org/maven2/com/jslsolucoes/ojdbc6/11.2.0.1.0/ojdbc6-11.2.0.1.0.jar

输入下面的命名（记得改成合适自己的版本号）

```
mvn install:install-file -Dfile=D:\\ojdbc6-11.2.0.3.jar -DgroupId=com.oracle -DartifactId=ojdbc6 -Dversion=11.2.0.3 -Dpackaging=jar

```

### 打包后的war包

在EasyReport\easyreport-web\target下

结果你发现是jar包，打开easyreport-web下的pom.xml
修改

```
<packaging>jar</packaging>
<artifactId>easyreport-web</artifactId>
<name>easyreport-web</name>
<description>controller与web视图模块</description>

```

```
<packaging>war</packaging>
<artifactId>easyreport-web</artifactId>
<name>easyreport-web</name>
<description>controller与web视图模块</description>

```