---
title: win10配置JAVA环境变量
categories: JAVA
tags: JAVA
keywords: JAVA
abbrlink: 6b40fbfe
date: 2018-06-19 16:15:00
---

在系统变量中添加

* JAVA_HOME    
```
C:\Program Files\Java\jdk1.8.0_144

```

* CLASSPATH

```
.;%JAVA_HOME%\lib\dt.jar;%JAVA_HOME%\lib\tools.jar;

```

* 编辑已存在的变量 Path

添加两行

```
%JAVA_HOME%\Bin


%JAVA_HOME%\jre\Bin

```
