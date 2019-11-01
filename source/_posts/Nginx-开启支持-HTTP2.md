---
title: Nginx 开启支持 HTTP2
tags:
  - nginx
  - web development
permalink: nginx-qi-yong-http2-2
id: 188
updated: '2017-06-17 14:54:03'
date: 2017-06-17 10:45:18
---

## HTTP2的好处

* Server Push 主动推送资源 （目前nginx还不支持，新版本nodeJs可以配置实现）
* 用帧二进制编码传输数据，连接可以承载任意数量的双向数据流 (所以不用关心，请求的数量了)
* 头部压缩，减少请求时间

## 版本要求

Nginx 1.9.5 及其以上版本

## 配置

其实配置很简单，只需要在原本配置的`listen`处加入 `http2` 的声明即可，而且对无法支持HTTP2的浏览器，nginx会做自动降级处理。
([Nginx 官方说明](https://www.nginx.com/blog/nginx-1-9-5/))

```
server {
    listen 443 ssl http2 default_server;

    ssl_certificate    server.crt;
    ssl_certificate_key server.key;
    ...
}
```

## 查看网站是否启用HTTP2

在chrome控制台的Network中开启Protocol即可查看请求类型
![](/images/2017/06/http2-1.png)

成功开启HTTP2，将会出现h2的标记

![](/images/2017/06/http2-2.png)

## 为什么会开启失败

几个月之前就将树莓派上的Nginx配置支持HTTP2，后来部分服务迁移到某国外服务器，直接将Nginx配置Copy过去却发现没有生效。

为什么会开启失败，首先第一个Nginx的version一定要对，在version正确的前提下，修改配置文件后，其实我们已经完成开启HTTP2。

但是如果Nginx所在操作系统OpenSSL Version为1.0.1x, Nginx基于这些版本 的OpenSSL编译，将会支持
`NPN`而不支持`ALPN`，而新版的Chrome 只支持`ALPN`，所以如果是这种情况在Chrome浏览器中HTTP2依然不会生效。

[Nginx官方说明](https://www.nginx.com/blog/supporting-http2-google-chrome-users/)

在[serverfault.com](https://serverfault.com/questions/775298/debian-jessie-nginx-with-openssl-1-0-2-to-use-alpn-rather-than-npn)也有网友给出了解决方案（debian 系统）

> This is a blockquote with two paragraphs. Lorem ipsum dolor sit amet,
> consectetuer adipiscing elit. Aliquam hendrerit mi posuere lectus.
> Vestibulum enim wisi, viverra nec, fringilla in, laoreet vitae, risus.
> 
> Donec sit amet nisl. Aliquam semper ipsum sit amet velit. Suspendisse
> id sem consectetuer libero luctus adipiscing.