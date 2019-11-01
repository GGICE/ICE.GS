---
title: 本地开发启用HTTPS
date: 2018-11-22 23:19:09
tags:
    - web development
---

现在众多网站或者浏览器的API都需要启用HTTPS，本地如果无法启用HTTPS就会有诸多开发的不便。

其实本地开发启用HTTPS只需要简单几步就可以搞定：

## 生成加密证书对

执行以下命令

```shell
openssl req -x509 -out localhost.crt -keyout localhost.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

其中的 `DNS:localhost` 字段中的`localhost`替换成通过host绑定到 `127.0.0.1` 的任意域名比如我们在host中设置了

```
# My hosts
127.0.0.1 dev.dev
```
那么就可以为`dev.dev` 这域名设置证书

```
openssl req -x509 -out localhost.crt -keyout localhost.key \
  -newkey rsa:2048 -nodes -sha256 \
  -subj '/CN=localhost' -extensions EXT -config <( \
   printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:dev.dev\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```


## 信任证书

命令执行完就会生成`localhost.crt` `localhost.key`，双击 `localhost.crt` 就会在`钥匙串` 中打开，选择导入证书（Mac OS 10.13 中会提示选择导入证书，10.14会直接导入），找到名为`localhost`的已导入证书后，设置信任，如下图

![](/images/2018/11/https-crt.png)

## 启用相应的服务器

以用`http-server`启动静态服务器为例

```
sudo http-server -S -K localhost.key -C localhost.crt -p 443
```

启动成功后用Chrome、Safari浏览器访问即可看到如下效果，Firefox 有点任性，访问起来还是会提示不安全。

![](/images/2018/11/https-open.png)


## 参考文章

https://letsencrypt.org/docs/certificates-for-localhost/