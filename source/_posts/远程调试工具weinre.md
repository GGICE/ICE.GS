---
title: 远程调试工具weinre
tags:
  - web development
permalink: yuan-cheng-diao-shi-gong-ju-weinre
id: 126
updated: '2014-12-10 20:00:35'
date: 2014-12-08 22:29:21
---

做移动端开发或者hybrid的开发可能在前端调试中会遇到这样两个场景:

1.在各种手机浏览器中调试，不是所有的浏览器都可以console.log出东西的。

2.在APP中，不是所有的APP都可以console.log出东西的。

那么我需要一个东西可以，可以console.log东西，不管前端在哪里运行，不不仅仅是这些，我们还需要一个强大的可以像在PC中调出console那么强大东西来。weinre就是这样的一个不错选择。

[官方网站](http://people.apache.org/~pmuellr/weinre-docs/latest/Home.html) （具体使用方法参见官方文档，写的很清楚。）

我做了一个公共的Weinre代理服务，任何人都可以通过[点我](http://weinre.ice.gs/),实现远程调试，而不需要自己搭建服务。


(其实weinre在上家公司做移动开发的时候已经而耳闻weinre,现在才意识到它的强大！赶脚老是跟不上步伐！)
