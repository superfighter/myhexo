---
title: mouseEvent
date: 2017-02-28 10:00:59
tags:
    - javascript
    - click
    - mouseEvent
---
# 事件类型
DOM3 级事件中定义了9个鼠标事件，分别是：

* click
* dbclick
* mousedown
* mouseenter
* mouseleave
* mousemove
* mouseout
* mouseover
* mouseup

## 区别

* mouseenter和mouseleave不冒泡，意味着鼠标移到目标元素的后代上不会触发事件响应

# click过程
* mousedown -> mouseup -> click
<!-- more -->
# 案例（Chrome V55）
* mouseenter和mouseleave在定义顺序上原生定义优先级高于JQuery定义级别，其他事件有先定义先执行原则。
````
	var p = document.querySelector('p')
	p.addEventListener('mouseenter',function(){
	  console.log('enter~~')
	},false)
	p.addEventListener('mouseout',function(){
	  console.log('out~~')
	},false)
	p.addEventListener('mouseleave',function(){
	  console.log('leave~~')
	},false)
	p.addEventListener('click',function(){
	  console.log('click~~')
	},false)
	$('p').mouseup(function(){
	  console.log('up')               
	}).mouseenter(function(){
	  console.log('enter')
	}).mouseout(function(){
	  console.log('out')
	}).mouseleave(function(){
	  console.log('leave')
	}).mousedown(function(){
	  console.log('down')
	// })
	}).click(function(){
	  console.log('click')
	})

	// 鼠标移到P上依次输出 enter enter~~
	// 鼠标移出P依次输出 out~~ out leave leave~~
	// 鼠标点击P依次输出 down up click~~ click
````