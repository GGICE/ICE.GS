---
title: Mac 10.10.3下使用Phonegap打包android程序
tags:
  - web development
  - soft
permalink: test
id: 134
updated: '2015-02-09 05:21:47'
date: 2015-01-22 22:27:06
---

最近想借鉴之前公司的HTML5架构，自己做一个可用的HTML5框架，实现简单的移动端应用开发。

最后一环的打包Android或者Ios程序自然就想到了Phonegap了。简单分享下运行Hello World的过程。

需要的东西有：JDK1.6(以上)，ant，node.js，Android sdk

######安装必要的工具：

1.JDK1.6（Mac 自带）。
2.ant

	brew install ant

3.node.js （[官方下载](http://nodejs.org/download/)）。
4.Android sdk ([官方下载](http://developer.android.com/sdk/installing/index.html)只需要下载SDK即可)。
安装： 将下载的文件解压到某个目录，打开tools目录下面的android。
如下图选择安装,既可以保证顺利打包所需的。
![](https://dn-icegsimg.qbox.me/android.png)
设置环境变量：

    vi ~/.bash_profile
	//加入以下内容 注意目录换成自己存放sdk的目录
	 export PATH=${PATH}:/Development/adt-bundle/sdk/platform-tools:/Development/adt-bundle/sdk/tools
更新环境变量：

    $ source ~/.bash_profile

5.Phonegap

	sudo npm install -g phonegap

######运行Hello world

    $ cordova create hello
    $ cd hello
    $ cordova platform add android
    $ cordova build

一切顺利的话，你将会看到打包好的apk程序。
