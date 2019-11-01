---
title: 模拟Contextmenu事件
tags:
  - web development
permalink: mo-ni-contextmenushi-jian
id: 178
updated: '2016-08-20 06:31:37'
date: 2016-08-20 06:15:35
---

## 新建一个鼠标事件并初始化


    var myEvt = document.createEvent('MouseEvents');
    myEvt.initMouseEvent(
      'click'          // event type
      ,true           // can bubble?
      ,true           // cancelable?
      ,window      // the event's abstract view (should always be window)
      ,1              // mouse click count (or event "detail")
      ,100           // event's screen x coordinate
      ,200           // event's screen y coordinate
      ,100           // event's client x coordinate
      ,200           // event's client y coordinate
      ,false         // whether or not CTRL was pressed during event
      ,false         // whether or not ALT was pressed during event
      ,false         // whether or not SHIFT was pressed during event
      ,false         // whether or not the meta key was pressed during event
      ,1             // indicates which button (if any) caused the mouse event (1 =      primary button)
     ,null          // relatedTarget (only applicable for mouseover/mouseout events)
    );


## 使用伪造的事件

当我们有了伪造的Event之后我们就可以去主动触发某个元素的的contentMenu事件，然后传入该Event。
当然不仅仅是contentMenu事件可以模拟触发。

## 拓展阅读

[document.createEvent](https://developer.mozilla.org/en-US/docs/Web/API/Document/createEvent)

[Creating_and_triggering_events](https://developer.mozilla.org/en-US/docs/Web/Guide/Events/Creating_and_triggering_events)
