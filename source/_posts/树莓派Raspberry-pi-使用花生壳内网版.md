---
title: 树莓派Raspberry pi 使用花生壳内网版
tags:
  - soft
  - raspberry pi
permalink: shu-mei-pai-raspberry-pi-shi-yong-hua-sheng-ke-nei-wang-ban
id: 154
updated: '2016-03-20 03:48:46'
date: 2015-08-27 06:41:03
---

可能花生壳推出针对Raspberry pi 客户端已经有一段时间，客户端支持内网版花生壳的。使用和安装也很简单。

1.下载解压安装包

    # wget http://hsk.oray.com/download/download?id=25 -O b.tgz
    # tar zxvf b.tgz

2.安装

    # cd phddns2/
    # ./oraynewph start

如果一切顺利的话，便会出现如下的提示：

    SN= *******
    Oraynewph start success

这就说明花生壳已经成功安装，并且运行了，记住你的SN码，因为登陆要用到。

3.管理内网映射

登陆 http://b.oray.com 输入你的SN码，绑定花生壳账号，即可进行内网映射。当然如果你想用自己的域名，想要稳定的服务，可能你需要购买花生壳的付费服务。

另外

可以映射到22端口，这样就可以在外网环境通过ssh登陆内网的raspberry pi。

enjoy!
