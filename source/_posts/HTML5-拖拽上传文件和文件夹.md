---
title: HTML5 拖拽上传文件和文件夹
tags:
  - web development
permalink: html5-tuo-zhuai-shang-chuan-wen-jian-he-wen-jian-jia
id: 145
updated: '2016-11-16 12:56:51'
date: 2015-05-12 00:12:28
---

拖拽动作会引发的一些事件：

     dragstart：网页元素开始拖动时触发。
     drag：被拖动的元素在拖动过程中持续触发。
     dragenter：被拖动的元素进入目标元素时触发，应在目标元素监听该事件。
     dragleave：被拖动的元素离开目标元素时触发，应在目标元素监听该事件。
     dragover：被拖动元素停留在目标元素之中时持续触发，应在目标元素监听该事件。
     drop：被拖动元素或从文件系统选中的文件，拖放落下时触发。
     dragend：网页元素拖动结束时触发。

##1.简单拖拽上传文件的实现

###1.1 建立可拖拽元素


    <div id="drop-zone">Drop files here</div>

###1.2 监听该元素的拖拽事件

    function handleDragOver(evt) {
        evt.stopPropagation();
        evt.preventDefault();
        evt.dataTransfer.dropEffect = 'copy'; // Explicitly show this is a copy.
    }
    var dropZone = document.getElementById('drop-zone');
    dropZone.addEventListener('dragover', handleDragOver, false);

###1.3 获得上传的文件

     function handleFileSelect(evt) {
        evt.stopPropagation();
        evt.preventDefault();
        var files = evt.dataTransfer.files; // FileList object.
    }
    dropZone.addEventListener('drop', handleFileSelect, false);

[简单上传文件的DEMO和源码查看](http://public.ice.gs/demo/drag/simpleUpFiles.html)

###1.4 上传文件的兼容性

    chrome 支持
    safari 支持
    IE10+  支持
    Firefox 支持

##2.拖拽上传文件夹(仅仅chrome支持)

与上传文件夹不同的是我们对handleFileSelect函数进行改造

    function handleFileSelect(evt) {
        evt.stopPropagation();
        evt.preventDefault();
        var files = [],
            items = evt.dataTransfer.items;

        function folderRead(entry){
            showFolder(entry);
            entry.createReader().readEntries(function (entries) {
                for(var i = 0; i < entries.length; i++){
                    var entry = entries[i];
                    console.log(entry);
                    if(entry.isFile){
                        entry.file(function(file){
                            show(file);
                        })
                    }else{
                        folderRead(entry);
                    }
                }
            });
        }


        for(var i = 0; i < items.length; i++){
            var entry = items[i].webkitGetAsEntry();
            if(!entry){
                return;
            }
            console.log(entry);
            if(entry.isFile){
                entry.file(function(file){
                    show(file);
                })
            }else{
                folderRead(entry);
            }
        }

        function show(f){
            var tpl = '<li><strong>'+ f.name + '</strong> || '
                + f.size +' bytes || last modified: ' +
                f.lastModifiedDate.toLocaleDateString() + '</li><li></li>';
            var lis = document.getElementsByTagName('li');
            lis[lis.length-1].innerHTML = tpl;
        }
        function showFolder(f){
            var tpl = '<li><strong>'+ f.name + '</strong></li><li></li>';
            var lis = document.getElementsByTagName('li');
            lis[lis.length-1].innerHTML = tpl;
        }
    }

通过items[i].webkitGetAsEntry()获取文件夹文件信息，然后通过递归调用遍历文件内容。

其中最终读取文件、文件夹信息是调用的HTML5的文件API。

[上传文件夹演示和源码查看](http://public.ice.gs/demo/drag/dragFolder.html)

##参考资料
<li>[File API 规范](http://www.w3.org/TR/file-upload/)
<li>[File Reader API 规范](http://www.w3.org/TR/file-upload/#dfn-filereader)
<li>[使用File API 读取文件](http://www.html5rocks.com/zh/tutorials/file/dndfiles/)
