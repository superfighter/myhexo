---
title: HTML5 naturalHeight naturalWidth
date: 2016-07-06 09:41:12
tags:
	- html5
	- naturalHeight
	- naturalWidth
---
部分内容参考[w3ctech](http://www.w3ctech.com/topic/112)
### 预加载 ###
* 设定图片自定义属性放置图片真实的URL，src放置默认打底图片
* 获取图片元素相对顶部的高度`imageClientTop`，如果滚动高度接近`imageClientTop` ，则设置图片的src属性为自定义属性的值，在图片onload时去除打底，展示真正的图片

* 在未知预加图片宽高时，要想办法获取图片的真实宽高，有两种方案：支持H5 naturalHeight的和不支持的
* 支持的
````
// 注意,这里也要在img onload状态后才能获取naturalWidth，动态插入直接读该属性可能返回0
var 
  nWidth = document.getElementById('example').naturalWidth,
  nHeight = document.getElementById('example').naturalHeight;
````
* 不支持的
````
  function getNatural (el) {
    var img = new Image();
    img.src = el.src;
    return {width: img.width, height: img.height};
  }

  var 
  natural = getNatural(document.getElementById('example')),
  nWidth = natural.width,
  nHeight = natural.height;
````
<!-- more -->
* jQuery自定义处理
````
(function($){
    var
    props = ['Width', 'Height'],
    prop;

    while (prop = props.pop()) {
    (function (natural, prop) {
      $.fn[natural] = (natural in new Image()) ? 
      function () {
      return this[0][natural];
      } : 
      function () {
      var 
      node = this[0],
      img,
      value;

      if (node.tagName.toLowerCase() === 'img') {
        img = new Image();
        img.src = node.src,
        value = img[prop];
      }
      return value;
      };
    }('natural' + prop, prop.toLowerCase()));
    }
  }(jQuery));

  // 如何使用:
  var 
  nWidth = $('img#example').naturalWidth(),
  nHeight = $('img#example').naturalHeight();
````
### Demo ###
````
	// 时间戳不读缓存
	var start_time = new Date().getTime();
	 
	// 图片地址
	var img_url = $(self.defaultConf.imgEl).eq(el).attr('origin') + '?v=' +start_time;
	 
	// 创建对象
	var img = new Image();
	 
	// 改变图片的src
	img.src = img_url;
	 
	// 定时执行获取宽高，获取宽高速度比onload要快
	var check = function(el){
	    // 只要任何一方大于0
	    // 表示已经服务器已经返回宽高

	    if(img.width>0 || img.height>0){
	        clearInterval(set);
	        //设置预加载图片宽高
	        setWidth(img);
	    }

	};
	 
	var set = setInterval(function(){check(el)},40);
	//加载失败状态展示
	img.onerror = function(){
	    var t = function(){
	        if($(self.defaultConf.imgEl).eq(el).parent('.imgWrapper')){
	            $(self.defaultConf.imgEl).eq(el)
	                .addClass('loaded')
	            clearInterval(cc);
	        }
	    }
	    var cc = setInterval(t,40);
	    return false;
	}
	// 加载完成获取宽高
	img.onload = function(){
	    var t = function(){
	        if($(self.defaultConf.imgEl).eq(el).parent('.imgWrapper')){
	            $(self.defaultConf.imgEl).eq(el).hide()
	                .attr('src',$(self.defaultConf.imgEl).eq(el).attr('origin'))
	                .addClass('loaded')
	                .fadeIn(10);
	            clearInterval(cc);
	        }
	    }
	    var cc = setInterval(t,40);
	};
````