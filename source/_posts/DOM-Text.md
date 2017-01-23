---
title: DOM Text
date: 2017-01-22 17:08:44
tags:
	- HTML
---
* 简述下innerText, textContent以及innerHTML的关系
* 共性很明了，三者都可以读写操作，获取/改变对应DOM节点的内容
* 差异体现以下几点：
	* 是否触发浏览器重绘
	* 是否安全
	* 是否对特殊元素屏蔽
	* 在页面生命周期里是否起破坏作用
* 大致可以从下表看出三者特性[[来源](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/textContent)]：

|               | innerHTML           | innerText  | textContent |
| ------------- |:-------------:|:-------------:|:-------------:|
| 触发重绘      | √ | √ | ×
| 包含script,style标签      |   √    | √(IE8↑)  | √
| hidden元素 |   √    |  √(IE8↑)   | √
| 永久影响 |   √    |  √   | √
| 避免XSS |   ×    |   √  | √
* 总结
	* innerText是IE引入的方法，所以会有兼容性问题，在IE9及以上版本同步了textContent的表现
