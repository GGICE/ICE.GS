---
title: JavaScript 内存管理和优化
tags:
  - web development
  - web development推荐方法
permalink: javascript-nei-cun-guan-li-he-you-hua
id: 170
updated: '2016-02-22 23:33:37'
date: 2016-02-22 23:30:36
---

### 内存的生命周期

1. 创建对象分配所需内存
2. 使用该对象
3. 当不被使用时释放

### 老的垃圾回收机制

老式的浏览器采用`引用计数算法垃圾回收`，对于循环引用的对象无法回收，已经基本废弃，这里不做讨论。

### 现代浏览器垃圾回收机制

现代浏览器采用`标记-清除算法垃圾回收`。这个算法，根据对象是否可以获得来确定是否回收。

算法设置一个根对象，定时从根对象开始，查找所有根开始引用的对象，然后逐级向下查找，确定所有可以获得对象和所有无法获得的对象。

这个算法解决了循环引用无法回收的问题，问题是无法被查询到的对象都会删除，可能存在小概率的误删。

### 主动内存回收

	var a = { some code }
	a = null //a对象内存将会被回收

对于一些mvc框架，会主动控制视图或者控制器的生命周期，完成业务逻辑和提高运行效率。

### 内存诊断工具

Chrome 的 Profiler

### V8 引擎的垃圾回收

[这里](http://alinode.aliyun.com/blog/14)有篇文章介绍v8引擎的垃圾回收机制

### 代码优化

#### 善用函数
使用匿名包裹页面，可以利于内存的回收。

	;(function() {
	  // 主业务代码
	})();

#### 避免全局变量
默认情况下，全局变量是不会被回收的。

#### 主动解除引用
对于，明确的业务流程可以在恰当的时间，主动解除变量引用来，使得被回收。

#### 良好的闭包管理
这里的largeStr 并不会被回收

	var a = function () {
	    var largeStr = new Array(1000000).join('x');
	    return function () {
	        return largeStr;
	    };
	}();

这里的largeStr 是会被回收的

	var a = function () {
	    var smallStr = 'x';
	    var largeStr = new Array(1000000).join('x');
	    return function (n) {
	        return smallStr;
	    };
	}();

所以在闭包的使用时，应该合理的设置返回值，并且关注返回值的回收。
