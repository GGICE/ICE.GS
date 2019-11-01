---
title: Mac 下制作Ubuntu安装盘
tags:
  - soft
permalink: mac-xia-zhi-zuo-ubuntuan-zhuang-pan
id: 147
updated: '2016-05-27 14:33:04'
date: 2015-05-12 06:54:44
---

为了装elementary os 0.3，查了一些将Ubuntu安装到移动硬盘的方法。最终找到一个在MAC比较简单的实现方法。

1.将下载的*.iso 转换成 DMG

     $ hdiutil convert -format UDRW -o ubuntu elementaryos-freya-amd64.20150411.iso
     //把当前目录下的elementaryos-freya-amd64.20150411.iso 转换成名位Ubuntu的dmg

2.插入U盘 解除挂载

     diskutil list
     //查看u盘挂载目录 可以看到类似 /dev/disk3
     diskutil unmountDisk /dev/disk3
     //解除挂载
3.将镜像写入U盘

     sudo dd if=ubuntu.img.dmg of=/dev/rdisk3 bs=1m

然后开机选择U盘启动就好了。
