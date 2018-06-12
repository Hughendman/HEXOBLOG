
---
title: node
date: 2018-06-09 11:15:00
categories : node
tags: node
keywords : node
---

# node文件整理

* 链接数据库（Mysql）   
```
var mysql = require('mysql');
var connection = mysql.createConnection({                         //链接数据库的配置
    host: 'localhost',                                                                      //IP
    user: 'root',                                                                              //用户名
    password: '123456',                                                              //密码
    database: 'test'                                                                       //数据库名
});
connection.connect();                                                               //开启链接
connection.query('SELECT 1 + 1 AS solution',                      //写入sql
function(error, results) {                                                            //返回的数据（result）报错信息（error）
    if (error) throw error;
    console.log('The solution is: ', results[0].solution)；
});
connection.end();                                                                      //关闭链接

```

*  [请求接口](http://blog.csdn.net/u012234915/article/details/54600132)

```
//请求musus接口
const request = require('request');
const hera_api = require('../config/hera');

exports.dv_dataset = function (req,res){
    res.status(200);
    var id= req.url.split("?")[1];

    request(hera_api.hera_api+'lineage/dataset?' + id , function (error, response, body) {
        console.log(hera_api.hera_api+'lineage/dataset?' + id);
        console.log(error);
        res.json(body);
    });
};
```

* 编写接口

```


var express=require('express');
var app =express();
var bodyParser = require('body-parser');
app.use(bodyParser.json({limit: '1mb'}));  //body-parser 解析json格式数据
app.use(bodyParser.urlencoded({            //此项必须在 bodyParser.json 下面,为参数编码
    extended: true
}));
//mysql
//mysql
const mysql = require('mysql');
const sql = require("../config/mysql");
const client = sql.config_sql;
exports.source_del =function (req,res) {
    var datas = req.body;
    var source_del_sql = "DELETE FROM dict_datasource WHERE id = " + datas.id;
    client.query(source_del_sql, (err,datas) => {
        res.status(200);
        if(err == null){
            res.json({
                status: true,
                data : datas
            });
        }else {
            res.json({
                status: false
            });
        }
});
};


```
* 在server.js中引入

```
/**
* Created by 尹雪松 on 2017/11/15.
*/

var express=require('express');
var app =express();
var bodyParser = require('body-parser');
app.use(bodyParser.json({limit: '1mb'})); //body-parser 解析json格式数据
app.use(bodyParser.urlencoded({ //此项必须在 bodyParser.json 下面,为参数编码
extended: true
}));
//设置跨域访问
app.all('*', function(req, res, next) {
res.header("Access-Control-Allow-Origin", "*");
res.header("Access-Control-Allow-Headers", "Origin, X-Requested-With, Content-Type, Accept");
res.header("Access-Control-Allow-Methods","PUT,POST,GET,DELETE,OPTIONS");
res.header("X-Powered-By",' 3.2.1');
res.header("Content-Type", "application/json;charset=utf-8");
next();
});
//引入外部文件接口
const jobData = require("./job/job");



app.post('/api/jobTable',jobData.job_data);//job table





//配置服务端口
var server = app.listen(9092, function () {
var host = server.address().address;
var port = server.address().port;
console.log('Example app listening at http://%s:%s';, host, port);
});

```
* [管理(pm2)](http://www.cnblogs.com/chyingp/p/pm2-documentation.html)

```
下载：npm -g i pm2 
查看日志： pm2 logs
监控： pm2 monit


```

    
    
```
启动： pm2 +nginx
upstream my_nodejs_upstream {
    server 127.0.0.1:3001;
}
server {
    listen 80;
    server_name my_nodejs_server;
    root /home/www/project_root;
    
    location / {
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X-NginX-Proxy true;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";
        proxy_max_temp_file_size 0;
        proxy_pass http://my_nodejs_upstream/;
        proxy_redirect off;
        proxy_read_timeout 240s;
    }
}


```

* 启动服务


```
var express=require('express');
var app =express();
//配置服务端口
var server = app.listen(9092, function () {
var host = server.address().address;
var port = server.address().port;
console.log('Example app listening at http://%s:%s';, host, port);
});


```
