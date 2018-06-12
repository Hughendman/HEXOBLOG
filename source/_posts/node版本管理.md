---
title: node版本管理
date: 2018-06-09 11:15:00
categories : node
tags: node
keywords : node
---

#### linux下使用nvm
```
curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

```


```
wget -qO- https://raw.githubusercontent.com/creationix/nvm/v0.33.1/install.sh | bash

```

> 安装完后，如果是用xshell连远程主机的话，先重连一次，不然会发现提示找不到nvm命令

```
source ~/.bashrc

```
列出所有版本号
```

nvm ls-remote


```

安装指定版本

```
nvm install v7.9.0

```

切换版本

```
nvm use v7.8.0

```

切换版本

```
nvm current

```

查看版本

```
nvm ls

```