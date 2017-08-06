---
title: iphone-click
date: 2016-04-18 22:32:29
tags: [javascript, iphone, css3]
---
### 问题描述 ###
---

* 通过Ajax异步获取城市信息，在回调方法中处理完字符串后append到指定容器里，绑定click事件
* 在chrome模拟移动端中按正确期望执行
* 在真机iphone6中确没有达到预期效果，原因在哪里呢？见下面分析。

<!-- more -->

### 问题代码 ###
---

```javascript
    <html>
        <head>test iphone click</head>
        <body>
        <div id="J_city"></div>
        </body>
        <script>
        $(function(){
            $('body').on('click','.j_city',function(){
                alert($(this).html());
            })    
            $.ajax({
                url:"xxx",
                data:'',
                success:function(data){
                    var html = '<ul>';
                    $.each(data,function(i,v){
                        html += '<li class="j_city" data-id="'+data.id+'">'+data.name+'</li>';
                    })
                    html += '</ul>';
                    $('#J_city').html($(html));
                }
            })
        })
        </script>
    </html>
```

### 分析 ###
-----

* 主要见[stackoverflow](http://stackoverflow.com/questions/3705937/document-click-not-working-correctly-on-iphone-jquery)

#### 方案一：CSS ####

```javascript
<style>
    .j_city 
    {
         cursor: pointer;
    }
</style>
```
```javascript
/iP/i.test(navigator.userAgent) && $('*').css('cursor', 'pointer');
```
* 原因就是里面说的:因为div,li等标签都是不可点击的标签，而a标签是clickable的标签。

`It's important to realize that if you're just using <a> tags everything will work as expected. You can click or drag by mistake on a regular <a> link on an iPhone and everything behaves as the user would expect.`

大意就是如果你用a标签做容器，那么就没有问题，你可以执行拖拽、点击行为，一切将如你所愿。

#### 方案二：定义touch事件 ####

```javascript
    $(document).ready(function () {
      init();
    });

    function fire(obj,event) { 
        // TODO something
        console.log(obj);
        event.preventDefault();
    }

    function touchHandler(event)
    {
        var touches = event.changedTouches,
            first = touches[0],
            type = "";

        switch(event.type)
        {
           case "touchstart": type = "mousedown"; break;
           case "touchmove":  type = "mousemove"; break;        
           case "touchend":   type = "mouseup"; break;
           default: return;
        }

        //initMouseEvent(type, canBubble, cancelable, view, clickCount, 
        //           screenX, screenY, clientX, clientY, ctrlKey, 
        //           altKey, shiftKey, metaKey, button, relatedTarget);

        var simulatedEvent = document.createEvent("MouseEvent");
        simulatedEvent.initMouseEvent(type, true, true, window, 1, 
                              first.screenX, first.screenY, 
                              first.clientX, first.clientY, false, 
                              false, false, false, 0/*left*/, null);
        //触发自定义事件
        first.target.dispatchEvent(simulatedEvent);
        if($(event.target).hasClass('j_provLi')){
            fire($(event.target),event);
        }
    }

    function init() 
    {
        document.addEventListener("touchstart", touchHandler, true);
        document.addEventListener("touchmove", touchHandler, true);
        document.addEventListener("touchend", touchHandler, true);
        document.addEventListener("touchcancel", touchHandler, true);    
    }
```
#### 方案三：借助第三方库 ####
---

* [FastClick](https://github.com/ftlabs/fastclick)
