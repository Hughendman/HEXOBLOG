---
title: JWT（JSON Web Tokens）
categories: server
tags: server
keywords: server
abbrlink: a13f356a
date: 2018-07-27 16:15:00
---

### JWT

互联网服务认证现在一种非常流行的方式就是服务器索性不保存session数据，所以数据都保存在客户端，每次请求都发挥服务器，JWT就是这种方案的一个代表

### 原理

服务器认证之后，生成一个JSON对象，发回给用户

```
{
    name: 'yinxs',
    email: '**'
}

```

在以后的通信中都需要带上这个对象

### 数据格式

JWT分为三个部分，如下

```
Header(头部)
Payload(负载)
Signature(签名)

```
也就是
Header.Payload.Signature


#### Header

Header是一个json对象，描述JWT的元数据，如下：

```
{
    "alg":"HS256",
    "typ":"JWT"
}

```

alg代表着签名的算法(algorithm),默认HMAC SHA256(写成HS256)；

typ属性表示这个令牌(token)的类型(type),JWT令牌统一写JWT


将上面的JSON对象使用Base64URL算法转成字符串

#### payload

Pauload用来存放实际需要传递的数据。JWT提供了7个官方字段，供选用：

```
iss (issuer)：签发人
exp (expiration time)：过期时间
sub (subject)：主题
aud (audience)：受众
nbf (Not Before)：生效时间
iat (Issued At)：签发时间
jti (JWT ID)：编号

```

但是你也可以定义私有字段，就像我一开始做的那样

注意：JWT默认是不加密的，任何人都可以读到，所以不要把私密信息放在这个部分

这个 JSON 对象也要使用 Base64URL 算法转成字符串

#### Signature

Signature部分是对前两部分的签名，防止数据篡改

首先需要指定一个密钥（secret）。这个密钥只有服务器知道，不能泄露给用户。然后使用Header里面指定签名算法（默认是 HMAC SHA256），按照下面的公式产生签名。
```
HMACSHA256(
    base64UrlEncode(Header) + "." +
    base64UrlEncode(payload) + "." +
    secret
)
```
算出签名以后，把 Header、Payload、Signature 三个部分拼成一个字符串，每个部分之间用"点"（.）分隔，就可以返回给用户

注： Base64URL

Base64URL跟 Base64 算法基本类似，但有一些小的不同。

JWT 作为一个令牌（token），有些场合可能会放到 URL（比如 api.example.com/?token=xxx）。Base64 有三个字符+、/和=，在 URL 里面有特殊含义，所以要被替换掉：=被省略、+替换成-，/替换成_ 。这就是 Base64URL 算法。


### 使用方法

客户端收到服务器返回的JWT，可以存储在Cookie中，也可以存储在localStroage。

另外的一种做法：跨域的时候，JWT就放在POST请求body中或者get请求的参数后面。

### 特点

* JWT 默认是不加密，但也是可以加密的。生成原始 Token 以后，可以用密钥再加密一次。
* JWT 不加密的情况下，不能将秘密数据写入 JWT。
* JWT 不仅可以用于认证，也可以用于交换信息。有效使用 JWT，可以降低服务器查询数据库的次数。
* JWT 的最大缺点是，由于服务器不保存 session 状态，因此无法在使用过程中废止某个 token，或者更改 token 的权限。也就是说，一旦 JWT 签发了，在到期之前就会始终有效，除非服务器部署额外的逻辑。
* JWT 本身包含了认证信息，一旦泄露，任何人都可以获得该令牌的所有权限。为了减少盗用，JWT 的有效期应该设置得比较短。对于一些比较重要的权限，使用时应该再次对用户进行认证。
* 为了减少盗用，JWT 不应该使用 HTTP 协议明码传输，要使用 HTTPS 协议传输。



