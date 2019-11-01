---
title: 通过 Webpack打包，了解不同的“模块定义”
tags:
  - web development
permalink: tong-guo-webpackda-bao-liao-jie-bu-tong-de-mo-kuai-ding-yi
id: 185
updated: '2017-05-27 02:48:30'
date: 2017-05-27 02:10:00
---

	
## webpack 打包输出方式

在webpack config中有两个属性

### `output.library`  设置打包输入的library名

### `output.libraryTarge` 设置打包方式

* `var`  var MyLibrary = _entry_return_; (默认)

* `this`  this["MyLibrary"] = _entry_return_;

* `window`  window["MyLibrary"] = _entry_return_;

* `global`  global["MyLibrary"] = _entry_return_;

* `commonjs` exports["Library"] = xxx

* `commonjs2` module.exports = xxx

* `amd`  Export to AMD

* `umd` Export to AMD, CommonJS2 or as property in root

  当配置为`this` `commonjs` 时而没有设置 `output.library` 打包时会复制导出对象的每个属性，即将每个属性都绑定到 this 或者 exports。

  `var` 和 `this` 方式适合在浏览器中直接运行，通过src方式引入的话，其实两者都是将Library帮到到了window。

  `commonjs`  适用于用标准commonjs引入

  `commonjs2` 适用于Node.js

  `amd` 适用于requireJs

  `umd` 同时支持了 AMD, CommonJS2, CommonJS 和 绑定到全局变量的方式（浏览器运行）

  会有类似下面代码做各种模块加载支持


         !function(e, t) {
           "object" == typeof exports && "object" == typeof module ? module.exports = t() :
           "function" == typeof define && define.amd ? define([], t) : 
           "object" == typeof exports ? exports.Iceter = t() : e.Iceter = t()
        }(this, function()) {}

  ## AMD、CommonJS、UMD

  ### AMD

  Asynchronous module definition (异步模块定义)，多用于浏览器端，典型的实现RequireJS。AMD可以异步加载模块，即便相互依赖的模块也可以分别加载，在所有依赖加载完成后运行，这对于浏览器来说不会发生阻塞，对页面加载有重要意义。

  ### CommonJS

  CommonJS是同步加载模块，NodeJS的module算是CommonJS的部分的实现。因为是同步加载所以不适用于浏览器端。

  Webpack里面提到的`commonjs` 完全符合CommonJS定义语法的输出，而像Node.js 用`module.export` 代替 `exports`并不完全符合CommonJS所以用`commonjs2`表示，这里只是Webpack对两种形式做的区分。

  > CommonJS 规范是为了解决 JavaScript 的作用域问题而定义的模块形式，可以使每个模块它自身的命名空间中执行。该规范的主要内容是，模块必须通过 `module.exports` 导出对外的变量或接口，通过 `require()` 来导入其他模块的输出到当前模块作用域中。

  ### UMD

  Universal Module Definition（通用模块定义）, 支持了 AMD, CommonJS2, CommonJS 和 绑定到全局变量的方式。

  ​


