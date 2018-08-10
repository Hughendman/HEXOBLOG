---
title: 手把手教你node发送邮件
categories: node
tags: node
keywords: node
abbrlink: cd60cfe6
date: 2018-07-20 16:15:00
---

## 使用插件nodemailer

```
cnpm i --save nodemailer

```

## CODE

```

const nodemailer = require('nodemailer');

let transporter = nodemailer.createTransport({
        service: '126',
        auth: {
            user: 'xuesong_yin@126.com',
            pass: 'y2001129' //授权码

        }
    });
    let mailOptions = {
        from: 'xuesong_yin@126.com', // 发送者
        to: 'yinxs@jointwisdom.cn', // 接受者,可以同时发送多个,以逗号隔开
        subject: 'YINXS博客', // 标题
        html: `<h2>YINXS博客</h2><p>http://hughendman.github.io</p>`
    };

    transporter.sendMail(mailOptions, function (err, info) {
        if (err) {
            console.log(err);
            return;
        }

        console.log('发送成功');
    });


```
![](https://raw.githubusercontent.com/Hughendman/picture/master/mail/1.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/mail/2.png)

## 发送附件

```
let transporter = nodemailer.createTransport({
        service: '126',
        auth: {
            user: 'xuesong_yin@126.com',
            pass: 'y2001129' //授权码

        }
    });
    let mailOptions = {
        from: 'xuesong_yin@126.com', // 发送者
        to: 'yinxs@jointwisdom.cn', // 接受者,可以同时发送多个,以逗号隔开
        subject: 'YINXS博客', // 标题
        html: `<h2>YINXS博客</h2><p>http://hughendman.github.io</p>`,
        attachments:[
            {
                filename : 'package.json',
                path: './package.json'
            },
            {
                filename : 'content',
                content : '发送内容'
            }
        ]
    };

    transporter.sendMail(mailOptions, function (err, info) {
        if (err) {
            console.log(err);
            return;
        }

        console.log('发送成功');
    });

```
![](https://raw.githubusercontent.com/Hughendman/picture/master/mail/3.png)
![](https://raw.githubusercontent.com/Hughendman/picture/master/mail/4.png)
## 注意

你的信息中在html里面不要包含https,不管是在p标签中还是在a标签中

## 参考

[nodejs模块nodemailer基本使用-邮件发送(支持附件)](https://blog.csdn.net/zzwwjjdj1/article/details/51878392)