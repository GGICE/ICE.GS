---
title: AMD、CMD 和 CommonJS的区别
tags:
  - web development
  - 模块加载器
permalink: requirejs-he-commonjsde-qu-bie
id: 142
updated: '2015-03-27 02:04:12'
date: 2015-03-27 01:31:07
---

#####AMD 与 CommonJS

Node.js的模块系统是参照CommonJs规范实现的，RequireJS是参照AMD规范实现的。

CommonJS使用exports对象来定义模块。

AMD中使用define,在模块加载之前声明依赖。

AMD更适合浏览器，因为它支持异步加载模块依赖。

requireJS在实现AMD的同时，还提供了一个CommonJS包裹，这样CommonJS模块可以几乎直接被RequireJS引入。

	define(function(require, exports, module) {
    var someModule = require('someModule'); // in the vein of node    
    exports.doSomethingElse = function() { return       someModule.doSomething() + "bar"; };
	});

#####AMD 与 CMD

SeaJS是参照CMD思想实现的。

知乎上面简洁明了的回答：

<table border="1">
<tr>
<th>方案</th>
<th>优势</th>
<th>劣势</th>
<th>特点</th>
</tr>
<tr>
<td>AMD</td>
<td>速度快</td>
<td>会浪费资源</td>
<td>预先加载所有的依赖，直到使用的时候才执行</td>
</tr>
<tr>
<td>CMD</td>
<td>只有真正需要才加载依赖</td>
<td>性能较差</td>
<td>直到使用的时候才定义依赖</td>
</tr>
</table>

[CMD定义规范](https://github.com/seajs/seajs/issues/242)

[AMD定义规范](https://github.com/amdjs/amdjs-api/wiki/AMD)

[SeaJS与RequireJS的不同](https://github.com/seajs/seajs/issues/277)
