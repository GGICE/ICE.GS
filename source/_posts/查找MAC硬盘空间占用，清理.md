---
title: 查找MAC硬盘空间占用，清理
tags:
  - Mac os X
  - os
permalink: cha-zhao-macying-pan-kong-jian-zhan-yong-qing-li
id: 179
updated: '2016-10-09 02:23:15'
date: 2016-08-20 06:39:52
---

## 2016年10月09日10:21:22 更新：

Macos sierra 10.12 版本之后，已经自带硬盘空间管理工具！

## OLD

之前在知乎上看到的方法，在每次128G MAC硬盘告急的时候，屡试不爽。

ncdu 是一个可以查看目录下所有文件文件夹大小，并提供完整统计结果的软件。所以我们可以根据ncdu的统计结果找出我们磁盘空间都被那些占用了，然后删除那些无关紧要的，回收空间。


1. 安装 ncdu

   brew install ncdu


2. 检索所有目录

    cd /    
    sudo ncdu


3. 等待结果

    可能需要经过漫长时间（几十分钟）的等待，ncdu会检索所有文件完成。
