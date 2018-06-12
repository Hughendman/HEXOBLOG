---
title: redis
date: 2018-06-09 11:15:00
categories : redis
tags: 
    - 服务器
    - redis
keywords : redis
---

### 下载

需要下载一个redis，或者是连接的那个服务器有redis

cnpm i redis

我的本机安装目录C:\Program Files\Redis

执行：redis-server.exe


```
//使用redis，用来缓存数据
const redis   = require('redis');
const client  = redis.createClient({host:'192.168.14.6', port: 6379,no_ready_check:true});
client.on("error", function (err) {
    console.log("redis client连接失败",err);
});
client.on('ready', function (res) {
    console.log('client ready');
});

client.on('connect', function () {
    client.set("var_2", "var_2_val", function () {
        var read_var_2=client.get("var_2");
        console.log("第二次读取到的值："+read_var_2);
    });
    client.quit();
});

```
这个里面设置了过期时间使用的redis.expire
```
router.get('/', function (ctx, next) {
	let render = null;
	render = tableData.tableData(tableJson);
    render.count = render.data.length;
    render.num = 10;
    redis.set('var_2', JSON.stringify(render));
    redis.expire('var_2', 300);
    render.data = render.data.slice(0,9);
    ctx.body = render;
});

```