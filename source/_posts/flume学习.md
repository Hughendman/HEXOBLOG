---
title: flume学习
categories: flume
tags: flume
keywords: flume
abbrlink: db6d0071
date: 2018-08-03 16:15:00
---

### Flume

来自于百度的介绍

Flume是Cloudera提供的一个高可用的，高可靠的，分布式的海量日志采集、聚合和传输的系统，Flume支持在日志系统中定制各类数据发送方，用于收集数据；同时，Flume提供对数据进行简单处理，并写到各种数据接受方（可定制）的能力。

当前Flume有两个版本Flume 0.9X版本的统称Flume-og，Flume1.X版本的统称Flume-ng。由于Flume-ng经过重大重构，与Flume-og有很大不同，使用时请注意区分。

### 功能

* flume 是一个分布式的，可靠的，可用的，非常有效率的对大数据量的日志数据进行收集，聚集，移动信息的服务。flume 仅支持在linux上面运行.
* flume 是一个基于流式数据，非常简单（就写一个配置文件就可以），灵活的架构，一个健壮的，容错的，简单的扩展数据模型用于在线上实时应用分析， 他的表现为：写一个source，channel，sink 之后一条命令就能够操作成功了。
* flume ， kafka 实时进行数据收集，spark , storm 实时去处理，impala 实时去查询。

### 安装

```
wget "http://mirrors.cnnic.cn/apache/flume/1.6.0/apache-flume-1.6.0-bin.tar.gz"
tar -xzvf apache-flume-1.6.0-bin.tar.gz
mv flume-1.6.0 /opt

```

### 修改配置文件

```
vim /opt/flume-1.6.0/conf/flume.conf

```

```
# 指定Agent的组件名称
a1.sources = r1
a1.sinks = k1
a1.channels = c1

# 指定Flume source(要监听的路径)
a1.sources.r1.type = spooldir
a1.sources.r1.spoolDir = /root/path

# 指定Flume sink
a1.sinks.k1.type = logger

# 指定Flume channel
a1.channels.c1.type = memory
a1.channels.c1.capacity = 1000
a1.channels.c1.transactionCapacity = 100

# 绑定source和sink到channel上
a1.sources.r1.channels = c1
a1.sinks.k1.channel = c1

```

### 启动flume agent

```
cd /opt/flume-1.6.0

bin/flume-ng agent --conf conf --conf-file conf/flume.conf --name a1 -Dflume.root.logger=INFO,console

```
### 使用

```
cp 1.log  /root/path/
```
### 参考

[Flume的下载安装](https://blog.csdn.net/lnho2015/article/details/52035145)