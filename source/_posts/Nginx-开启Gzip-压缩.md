---
title: Nginx 开启Gzip 压缩
tags:
  - nginx
  - soft
permalink: nginx-kai-qi-gzip-ya-suo
id: 162
updated: '2016-02-18 04:35:09'
date: 2015-10-29 04:31:51
---

Nginx 开启Gzip:

打开Nginx 配置文件,添加如下内容:

    gzip on;
    gzip_min_length 1k; #要压缩的最小文件大小
    gzip_buffers 4 16k; #压缩缓冲大小设置
    gzip_http_version 1.0; #注释1
    gzip_comp_level 2;  #压缩级别,1-10，数字越大压缩的越好，打开时间也越长
    gzip_types text/plain application/x-javascript text/css application/xml      text/javascript application/x-httpd-php image/jpeg image/gif image/png; #要压缩的文件类型
    gzip_vary off; #注释2
    gzip_disable "MSIE [1-6]\."; #IE6不支持Gzip,不给它压缩



   注释1 :

它的默认值是1.1，就是说对HTTP/1.1协议的请求才会进行gzip压缩
如果我们使用了proxy_pass进行反向代理，那么nginx和后端的upstream server之间是用HTTP/1.0协议通信的
     This module makes it possible to transfer requests to another server.
     It is an HTTP/1.0 proxy without the ability for keep-alive requests yet. (As a result, backend connections are created and destroyed on every request.) Nginx talks HTTP/1.1 to the browser and HTTP/1.0 to the backend server. As such it handles keep-alive to the browser.
     如果我们使用nginx通过反向代理做Cache Server，而且前端的nginx没有开启gzip
     同时，我们后端的nginx上没有设置gzip_http_version为1.0，那么Cache的url将不会进行gzip压缩

注释2:

和http头有关系，加个vary头，给代理服务器用的，有的浏览器支持压缩，有的不支持，所以避免浪费不支持的也压缩，所以根据客户端的HTTP头来判断，是否需要压缩


[官方文档 Module ngx_http_gzip_module](http://nginx.org/en/docs/http/ngx_http_gzip_module.html)
