---
title: Let's Encrypt 免费好用的 HTTPS 证书 （Raspberry pi）
tags:
  - linux
  - soft
permalink: lets-encrypt-mian-fei-hao-yong-de-https-zheng-shu-raspberry-pi
id: 181
updated: '2017-05-16 15:33:04'
date: 2016-10-08 13:24:36
---

Let's Encrypt 被称为个人HTTPS的最佳解决方案，得到Mozilla、CISCO、Chrome、Facebook 等众多企业组织的支持。Let's Encrypt 也提供一套简单的HTTPS配置方案。

![](/content/images/2016/10/-----2016-10-09---12-12-37.png)

本文简单分享下安装 Let's Encrypt HTTPS证书过程，机器为Raspberry pi 3 ，系统为 Debian7，服务器软件为Nginx。

## 安装 cerbot-auto
cerbot-auto 是 Let's Encrypt 的自动化安装工具。

    wget https://dl.eff.org/certbot-auto  
    chmod a+x certbot-auto

## 使用
待安装完成之后

    ./certbot-auto certonly --standalone -d x21.xyz -d center.x21.xyz

-d 之后是跟随配置域名，子域名可使用多个-d ，Let's Encrypt 不支持泛域名所以要设置每一个子域名, 回车后将会出现图形界面，输入邮箱等设置信息。

注意： 使用前记得停掉Nginx，其他占用80或者443端口的程序也需要停掉，certbot-auto 认证需要这两个端口。

## 配置nginx

center.x21.xyz 域名的配置实例

    server {
        listen 80;
        listen [::]:80;
        server_name center.x21.xyz;
        return 301 https://$host$request_uri;
    }
    server {
    	listen 443 ssl;
        listen [::]:443 ssl;
    	server_name center.x21.xyz;

    	ssl_certificate /path/fullchain.pem; #生成的fullchain.pem 路径
        ssl_certificate_key /path/privkey.pem; #生成的privkey.pem 路径
        ssl_session_timeout 1d;
        ssl_session_cache shared:SSL:50m;
        #ssl_session_tickets off;

    	ssl_dhparam /etc/nginx/ssl/dhparam.pem;
        # dhparam.pem 生成方法
        # $ sudo mkdir /etc/nginx/ssl
        # $ sudo openssl dhparam -out /etc/nginx/ssl/dhparam.pem 2048

    	ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
    	ssl_ciphers 'ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256:ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:DHE-RSA-A    ES128-GCM-SHA256:DHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-AES128-SHA256:ECDHE-RSA-AES128-SHA256:ECDHE-ECDSA-AES128-SHA:ECDHE-RSA-AES256-SHA384:ECDHE-RSA-AES128-SHA:ECDHE-ECDSA-AES256-SHA384:ECDHE-ECDSA-A    ES256-SHA:ECDHE-RSA-AES256-SHA:DHE-RSA-AES128-SHA256:DHE-RSA-AES128-SHA:DHE-RSA-AES256-SHA256:DHE-RSA-AES256-SHA:ECDHE-ECDSA-DES-CBC3-SHA:ECDHE-RSA-DES-CBC3-SHA:EDH-RSA-DES-CBC3-SHA:AES128-GCM-SHA256:    AES256-GCM-SHA384:AES128-SHA256:AES256-SHA256:AES128-SHA:AES256-SHA:DES-CBC3-SHA:!DSS';
    	ssl_prefer_server_ciphers on;

    	add_header Strict-Transport-Security max-age=15768000;
    	ssl_stapling on;
        ssl_stapling_verify on;

    	resolver 8.8.8.8 8.8.4.4;

    	location / {
            	proxy_set_header   X-Real-IP $remote_addr;
            	proxy_set_header   Host      $http_host;
            	proxy_pass         http://127.0.0.1:9901;
    	}

    }

## HTTPS 配置文件生成器

Mozilla 有一个HTTPS配置文件的生成器[Mozilla SSL Configuration Generator](https://mozilla.github.io/server-side-tls/ssl-config-generator/)

上面的nginx便是配置借鉴Mozilla SSL Configuration Generator，但是nginx version: nginx/1.6.2, 一些特性并不支持，删减了一些配置 (一些nginx配置并不了解用途😭)。

## 测试HTTPS配置，评分
[Qualys SSL Labs](https://www.ssllabs.com/ssltest/index.html)提供了全面的 SSL 检测和评级，可以根据评测报告修改HTTPS配置。

上面的配置已经获得了A+

![](/content/images/2016/10/-----2016-10-09---12-14-31.png)

## 自动更新证书

Let's Encrypt 证书的期限是3个月，到期之后需要续期，借助cerbot-auto工具可以实现自动更新。

使用 crontab 配置自动任务

    sudo crontab -e

添加以下内容

    56 0,3 * * * /usr/bin/certbot renew --quiet --no-self-upgrade

每天0点和3点执行一次更新证书命令 （cerbot-auto 推荐每天检查两次）
