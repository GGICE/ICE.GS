---
title: 基于JWT的登录验证系统实现
tags:
  - web development
permalink: ji-yu-jwtde-deng-lu-yan-zheng-xi-tong-shi-xian
id: 186
updated: '2017-06-02 06:14:30'
date: 2017-06-01 12:31:29
---

## 概述

 基于JWT的简单登录认证流程图：

![](/images/2017/06/jwt-1.png)

## JWT

> JSON Web token（JWT）是一种开放标准（RFC 7519），它定义了一种紧凑且独立的方式，用于将各方之间的信息安全地传输为JSON对象。该信息可以通过数字签名进行验证和信任。使用密匙 （使用哈希算法）或使用RSA的公钥/私钥对可以对JWT进行签名。

jwt是对于token认证方式的一种规范和描述。

#### JWT 的构成

jwt由三部分构成，并用`.`作为间隔：

* Header
* Payload
* Signature

看起来像这样 `xxxxx.yyyyy.zzzzz`

##### Header

Header用来描述token类型和加密算法(HMAC、SHA256、RSA 等)，如下：

```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

##### Payload

这部分用来放置token的有效信息，有`reserved，public，private`三种类型：

* **Reserved claims：** 一些保留类型的，推荐但不强制必须使用，包括以下属性
  * **iss**: jwt签发者
  * **sub**: jwt所面向的用户
  * **aud**: 接收jwt的一方
  * **exp**: jwt的过期时间，这个过期时间必须要大于签发时间
  * **nbf**: 定义在什么时间之前，该jwt都是不可用的.
  * **iat**: jwt的签发时间
  * **jti**: jwt的唯一身份标识，主要用来作为一次性token,从而回避重放攻击。
* **Public claims：** 可以随意定义的属性, 但为了避免冲突，他们应该使用 IANA JSON Web Token Registry 定义或使用 URL 定义。
* **Private claims:** 这些是为了在同意使用它们的各方之间共享信息而创建的自定义声明。

一个Payload看起来像这样

```json
{
  "sub": "1234567890",
  "name": "John Doe",
  "admin": true
}
```

##### Signature

将Header、Payload编码后加上secret，并用Header声明的算法进行加密，看起来伪代码的实现是这样的

```javascript
HMACSHA256(
  base64UrlEncode(header) + "." +
  base64UrlEncode(payload),
  secret)
```

##### 把三部分组合起来

最终的样子像这样

```javascript
eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJzdWIiOiIxMjM0NTY3ODkwIiwibmFtZSI6IkpvaG4gRG9lIiwiYWRtaW4iOnRydWV9.TJVA95OrM7E2cBab30RMHrHDcEfxjoYZgeFONFh7HgQ
```

## 实现登录认证

#### 服务端

[jwt.io](jwt.io)网站提供众多后端语言的支持库，帮我们封装好了jwt的一些具体实现，其中包括Node.js的[node-jsonwebtoken](https://github.com/auth0/node-jsonwebtoken) 。

##### 生成token

`jwt.sign(payload, secretOrPrivateKey, [options, callback])`

`jwt.sign` 方法中的`secretOrPrivateKey`即为JWT第三部分`Signature`中的secret, 它既可以是一个复杂的字符串，也可以是一对密钥对中的`PrivateKey`。

```javascript
var privateKey = fs.readFileSync(config.privateKey)
var token = jwt.sign({
	user: oneUser._id,
	exp: Math.floor(Date.now() / 1000) + (30) //30s后过期
}, privateKey, { algorithm: 'RS256'})
```

privatekey生成

```
ssh-keygen -t rsa -b 4096 -f private.key #生成私钥
# Don't add passphrase
openssl rsa -in private.key -pubout -outform PEM -out public.key #生成对应公钥
# 生成公钥格式需要是PEM格式的, ss-keygen 也可以生成公钥
# ssh-keygen -f public.key -e -m pem  
cat private.key
cat public.key
```

##### 验证token

`jwt.verify(token, secretOrPublicKey, [options, callback])`

```javascript
var publicKey = fs.readFileSync(config.publicKey)
try {
  result = jwt.verify(token, publicKey) 
} catch(e) {}
```

#### 客户端

在每次发起需验证请求时在头部携带JWT格式的token

```
Authorization: Bearer <token>
```

### 优点

> - 体积小，因而传输速度快
> - 传输方式多样，可以通过 URL/PosT 参数/HTTP 头部 等方式传输
> - 严谨的结构化。它自身（在 payload 中）就包含了所有与用户相关的验证消息，如用户可访问路由、访问有效期等信息，服务器无需再去连接数据库验证信息的有效性，并且 payload 支持为你的应用而定制化
> - 支持跨域验证，多应用于单点登录。
> - 充分依赖无状态 API ，契合 RESTful 设计原则
> - 验证解耦，无需使用特定的身份验证方案，易于实现分布式
> - 比 cookie 更支持原生移动端应用

## 单点登录实现
#### 登录

基于jwt的验证方式实现单点登录，只需要在单点（暂时称之为center）登录后通过一定方式将token分发给其他点。

token的验证可以在center端统一处理，也可将上文提到的`publicKey`分发给各个点，各个点自行验证。

token的各个点存储可以通过cookie、localstorage。

![](/images/2017/06/jwt-2.png)

#### 退出登录

退出登录实现和登录类似，请求center退出登录页面，center清理各个点的cookie或者localstorage实现退出登录。
## 参考资料

[5 Easy Steps to Understanding JSON Web Tokens (JWT)](https://medium.com/vandium-software/5-easy-steps-to-understanding-json-web-tokens-jwt-1164c0adfcec)

[Introduction to JSON Web Tokens](https://jwt.io/introduction/)