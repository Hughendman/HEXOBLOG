---
title: linux定时任务(crontab)
categories: linux
tags: linux
keywords: linux
abbrlink: c156bde1
date: 2018-06-29 16:15:00
---


## crontab 添加任务

crontab -e

### 规则

```
分 时 日 月 周
 *  *  *  *  *  sh 1.sh
 0-59  0-23  1-31  1-12  0-7  需要执行的命令
```
在以上各个字段中，还可以使用以下特殊字符：

星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。

逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”

中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”

正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。


### crond服务

安装crontab：

yum install crontabs

服务操作说明：
```

重启： /etc/init.d/crond restart

查看日志： cat /var/log/cron

修改： crontab -e

```

### 问题

你可能会发现的脚本被执行了多层次


```
 ps -A | grep cron 

```

查看进程，是否启动多个，kill掉