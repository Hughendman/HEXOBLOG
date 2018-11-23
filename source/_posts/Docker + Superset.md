---
title: Docker + Superset
categories: Superset
tags: Superset
keywords: Superset
abbrlink: 4fbb2b9c
date: 2018-10-11 16:15:00
---

### 安装docker

```
yum install epel-release

yum install docker-io

```

### 启动doker

```
service docker start

```

### 安装superset


```
#查找镜像
docker search superset  

 
#拉取镜像
docker pull amancevice/superset:0.27.0 

#查看镜像
docker images   


# 创建容器
docker run -d -p 9300:8088 -v /dockersuperset:/home/superset amancevice/superset:0.27.0

#获取CONTAINER ID
docker ps     

#以下步骤为superset初始化步骤
docker exec -it CONTAINER ID  fabmanager create-admin --app superset


docker exec -it CONTAINER ID superset db upgrade


superset load_examples


docker exec -it CONTAINER ID superset init


docker exec -it CONTAINER ID superset runserver



```

访问：
http://localhost:9033/superset/welcome