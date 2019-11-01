---
title: ubuntu 下安装 photoshop cs6 || cc
tags:
  - linux
  - soft
permalink: ubuntu-xia-an-zhuang-photoshop-cs6
id: 176
updated: '2016-05-07 07:40:27'
date: 2016-05-07 07:25:09
---

根据wine官网的说明，在wine1.9.8 的版本是可以安装photoshop cc的。[这里](https://appdb.winehq.org/objectManager.php?sClass=version&iId=29832&iTestingId=83387)有官方的安装说明。

![](/content/images/2016/05/2016-05-07-15-38-15-----.png)
## 安装篇

1. 安装wine 1.9

        sudo add-apt-repository ppa:wine/wine-builds
        sudo apt-get update
        sudo apt-get install --install-recommends wine-staging
        sudo apt-get install winehq-staging


之后可以按照官方的说法继续


1. Installed "Adobe Application Manager" from http://www.adobe.com/support/downloads/detail.jsp?ftpID=4773
2. Started Adobe Application Manager -> Waiting for the update -> Login with adobe account -> Shows only Photoshop CS6.
3. Closed all instanced, run winecfg, set os to "Windows 7" (was XP on default)下载好
4. Started the Application Manager -> Now shows Photoshop CC
5. Installed Photoshop CC Via Application Manager
(Optional components fail to install - however photoshop runs perfectly without that)
6. Start Photoshop.exe with wine.

我是直接，在第2. 直接安装了Photoshop CS6 因为个人来讲cs6 已经够我用的了，Adobe Application Manager 会直接把cs6下载好然后自动安装好，之后就可以在wine菜单中找的cs6的图标，直接点击就可以打开。

## 激活篇

   其实激活方法和Window找到cs6安装目录，然后...
