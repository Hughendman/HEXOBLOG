---
title: 压力测试(siege)）
categories: 测试
tags: 测试
keywords: 测试
abbrlink: 448930f9
date: 2018-07-31 16:15:00
---

## 安装

### 下载
[下载地址](http://download.joedog.org/siege/)

### 安装命令
```
tar -xzvf siege-2.70.tar 
cd siege-2.70
./configure
make 
make install

```
### 验证

```
siege -version

```

## get

```
siege -c 200 -r 100 http://192.168.19.5:11111/hotel/word?hotelname=%E5%8C%97%E4%BA%AC%E9%A5%AD%E5%BA%97&amp;num=5

```

返回：

```
ransactions:		       19509 hits  //访问次数
Availability:		       97.55 %     //成功次数
Elapsed time:		      553.68 secs  //测试用时
Data transferred:	        8.87 MB    //测试传输数据量
Response time:		        4.84 secs  //平均响应时间
Transaction rate:	       35.24 trans/sec  //每秒事物处理量
Throughput:		        0.02 MB/sec    //吞吐率
Concurrency:		      170.51       //并发用户数
Successful transactions:       19509   //成功传输次数
Failed transactions:	         491   //失败传输次数
Longest transaction:	       28.75   //最长响应时间
Shortest transaction:	        0.04   //最短响应时间

```

## post

```
siege "http://192.168.14.6:3100/route/hotel/word POST {"access_token":"eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJuYW1lIjoi5bC56Zuq5p2-IiwicHdkIjoiYWJjZDIzNCIsImVtYWlsIjoieHVlc29uZ195aW5AMTI2LmNvbSIsInRlbGVwaG9uZSI6IjE4ODExNDI4NDUyIiwiaWF0IjoxNTMxODA1Njg5fQ.yVqCj26HeB3MAwbqoXyOdvz9-9vY_gCCwdUB1F60I3w","hotelname":"北京饭店","num":5}" -r 1000 -c 200
```

返回：

```
ransactions:		       20000 hits
Availability:		      100.00 %
Elapsed time:		       66.88 secs
Data transferred:	        1.74 MB
Response time:		        0.02 secs
Transaction rate:	      299.04 trans/sec
Throughput:		        0.03 MB/sec
Concurrency:		        5.43
Successful transactions:       20000
Failed transactions:	           0
Longest transaction:	        0.49
Shortest transaction:	        0.00

```
### 日志

想要打上日志

```
创建：/usr/local/var/siege.log

```
