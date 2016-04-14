---
title: Media query
date: 2016-04-14 08:37:30
tags: css3 media
---
## Media Query ##

> `内容部分参照于`[W3C Plus](http://www.w3cplus.com/content/css3-media-queries)

### 含义 ###

* Media Query 直译就是"媒体查询"，常见于检测展示页面所属终端，针对不同终端赋予不同样式。

### 代码片段 ###

片段A

    <link href="css/reset.css" rel="stylesheet" type="text/css" media="screen" />
    <link href="css/style.css" rel="stylesheet" type="text/css" media="all" />
    <link href="css/print.css" rel="stylesheet" type="text/css" media="print" />

片段B

	  <style type="text/css" media="screen">
	    @import url("css/style.css");
	  </style>

片段C

	<link rel="stylesheet" media="screen and (max-width: 600px)" href="small.css" />

片段D

	  @media screen,3D and (max-width: 600px) {
    	选择器 {
      		属性：属性值；
    	}
  	  }

### 分析代码 ###

#### 共性 ####

* 以上代码片段都是Media query的使用场景
	* `link`方式引入和`@import`方式引入
	* `@media`方式引入
* 属性介绍
	* 媒介类型(常用三种，齐全[10种](https://www.w3.org/TR/CSS2/media.html#media-types),如3D,TV,handheld等.)
		* `print` 打印
		* `screen` 屏幕
		* `all` 所有
	* 关键词
		* `and`
		* `not`
		* `only`
			* only用来定某种特定的媒体类型，可以用来排除不支持媒体查询的浏览器。其实only很多时候是用来对那些不支持Media Query但却支持Media Type的设备隐藏样式表的。其主要有：支持媒体特性（Media Queries）的设备，正常调用样式，此时就当only不存在；对于不支持媒体特性(Media Queries)但又支持媒体类型(Media Type)的设备，这样就会不读了样式，因为其先读only而不是screen；另外不支持Media Qqueries的浏览器，不论是否支持only，样式都不会被采用
	* 媒体特性(常见width,更多见[Media features](https://www.w3.org/TR/css3-mediaqueries/#media1))
		* `max-width`
		* `min-width`
		* `max-device-width`
		* `min-device-width`

#### Media query和Css 语法结构区别 ####

* Media query只接受单个的逻辑表达式作为值，或者没有值
* css属性用于声明如何表现；Media query是一个用于判断设备是否满足某种条件的表达式
* Media query中大部分接受min/max前缀，用来表示其逻辑关系，表示应用于大于等于或者小于等于某个值的情况
* css属性要求必须有属性值，Media query可以没有值，因为其表达式返回的只有真或者假

#### 单例 ####

iphone4

       <link rel="stylesheet" media="only screen and (-webkit-min-device-pixel-ratio: 2)" type="text/css" href="iphone4.css" />

ipad

 	 <!-- 竖横屏 -->	
	 <link rel="stylesheet" media="all and (orientation:portrait)" href="portrait.css" type="text/css" /> 
	 <link rel="stylesheet" media="all and (orientation:landscape)" href="landscape.css"  type="text/css" />