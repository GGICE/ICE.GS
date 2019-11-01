---
title: gulp 顺序执行任务
tags:
  - web development
permalink: gulp-shun-xu-zhi-xing-ren-wu
id: 160
updated: '2015-09-08 06:55:38'
date: 2015-09-08 06:27:34
---

默认情况下gulp任务是并行执行的。

串行执行有几种方法

1.用callback的方式

    var gulp = require('gulp');

    // 传入一个回调函数，因此引擎可以知道何时它会被完成
    gulp.task('one', function(cb) {
         // 做一些事 -- 异步的或者其他任何的事
         cb(err); // 如果 err 不是 null 和 undefined，流程会被结束掉，'two' 不会被执行
    });

    // 标注一个依赖，依赖的任务必须在这个任务开始之前被完成
    gulp.task('two', ['one'], function() {
       // 现在任务 'one' 已经完成了
    });

2.返回steam

    var gulp = require('gulp');
    var del = require('del'); // rm -rf

    gulp.task('clean', function(cb) {
        del(['output'], cb);
    });

    gulp.task('templates', ['clean'], function() {
        var stream = gulp.src(['src/templates/*.hbs'])
        // 执行拼接，压缩，等。
        .pipe(gulp.dest('output/templates/'));
        return stream; // 返回一个 stream 来表示它已经被完成
    });

    gulp.task('styles', ['clean'], function() {
        var stream = gulp.src(['src/styles/app.less'])
        // 执行一些代码检查，压缩，等
        .pipe(gulp.dest('output/css/app.css'));
        return stream;
    });

    gulp.task('build', ['templates', 'styles']);

    // templates 和 styles 将会并行处理
    // clean 将会保证在任一个任务开始之前它完成
    // clean 并不会被执行两次，尽管它被作为依赖调用了两次

    gulp.task('default', ['build']);

问题：为什么要返回steam？

虽然在gulp.task('build', ['templates', 'styles']);数组中已经声明了依赖关系，但是这个依赖关系是只能保证前一个函数执行完毕，而它的文件操作等可能还未完成，所以需要以steam结束为依据。

问题：1个task中有多个文件等steam操作，如何保证内部并行执行，并且监听所有steam均结束的状态？

借助[event-steam](https://www.npmjs.com/package/event-stream)等工具，监听steam的结束。

参考资料：

http://www.gulpjs.com.cn/docs/recipes/running-tasks-in-series/

http://www.lifelaf.com/blog/?p=1210

https://www.npmjs.com/package/event-stream
