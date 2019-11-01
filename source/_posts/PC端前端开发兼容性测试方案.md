---
title: PC端前端开发兼容性测试方案
tags:
  - web development
permalink: pcduan-qian-duan-kai-fa-jian-rong-xing-ce-shi-fang-an
id: 123
updated: '2014-11-18 20:04:33'
date: 2014-11-18 00:19:43
---

近3个月百度统计的浏览器市场份额（360浏览器被“分割”为对应内核的浏览器）

![](https://dn-icegsimg.qbox.me/ie.jpg)

做PC开发已经有两年多了，PC端的兼容性问题在之前一直受IE6的困扰，变得非常困难。如今已经越来越多的公司包括国内公司放弃支持IE6,之前我做兼容性测试的时候是虚拟机xp + ie6 、虚拟机win7 + ie8 、虚拟机win7 + ie9、 win8.1+ie11。

如今，环境已经改变，我们可以只做兼容到IE8的处理，我个人也非常赞成这么做。

######比较简单的兼容性测试方案：

Ietester + IE11

总所周知Ietester，可以做查看网站在IE版本下的情况，但是它并不准确，只是可以粗略的显示一下情况。

IE11，F12开发者工具中可以调整模拟IE浏览器版本，它比Ietester能更准确的显示真实的状况。

######建议的解决方案：

我们考虑到兼容性问题的话，个人建议只需要考虑这几种浏览器：Chrome 、 Firefox 、safari、IE11、IE9、IE8。

虽然不同操作系统间的相同浏览器也会出现不同的渲染表现，但是这种情况同样非常少。所以在建议解决方案中，只考虑浏览器版本。

Chrome、Firefox、safari你可以在一个系统中拥有他们。
IE11、IE9、IE8 可以在 [modern.IE](https://www.modern.ie/zh-cn/virtualization-tools#downloads)中下到带有不同版本IE浏览器的虚拟机系统。

######严谨的解决方案：

目前[modern.IE](https://www.modern.ie/zh-cn/virtualization-tools#downloads)提供9种操作系统和IE浏览器的组合，可以安装这9个虚拟机，分别测试9中环境下的兼容性问题。

然后再加上Chrome、Firefox、safari的兼容性测试。

######国内情况

cnzz 8月份浏览器市场份额的统计

![](https://dn-icegsimg.qbox.me/iecnzz.png)

可以看出360浏览器还占据很大份额，搜狗、腾讯、2345、猎豹、遨游等也占有一定的市场份额，虽然它们大多数内核是基于以上所说的主流浏览器，可是非常不幸的是，在你确定兼容所以主流浏览器的时候，可以你在这些国产浏览器中并不能正确的渲染。

所以，我们有时候还需调整对这些国产浏览器的兼容性，起码市场份额较大的360和搜狗我们要考虑完美兼容它们。
