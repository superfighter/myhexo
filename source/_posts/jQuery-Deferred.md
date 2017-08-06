---
title: jQuery-Deferred
date: 2016-05-31 14:50:24
tags:
	- jQuery
	- ajax
---

* jQuery 1.5.0 之前的jQuery调用ajax语法

````
　　			$.ajax({
　　　　			url: "test.html",
　　　				success: function(){
　　　　　　				alert("success!");
　　　　			},
　　　　			error:function(){
　　　　　　				alert("error!");
　　　　			}
　　			});
````

* jQuery 1.5.0 新增Deferred对象后ajax语法

````
　　			$.ajax({url: "test.html"}).done(function(){alert("success!")}).fail(function(){alert("error!")});
````

* 多次回调

````
			$.ajax({url: "test.html"}).done(function(){alert("success!")}).fail(function(){alert("error!")}).done(function(){alert("success again")});
````

* 为多个事件制定一个回调参数

````
	$.when($.ajax({url:'1t.html'}),$.ajax({url:'2t.html'})).
		done(function(){alert('all success')}).
		fail(function(){alert('not all success')});
````

* 普通函数的回调参数

````
		//判断是否支持webP格式图片
		function hasWebP(){
			var rv = $.Deferred();
			var img = new Image();
			img.onload = function (){ return rv.resolve();}
			img.onerror = function(){ return rv.reject();}
			//img.src="http://baidu.com/1.jpg";
			img.src="http://musicdata.baidu.com/data2/pic/39817702/39817702.jpg";
			$('div').append(img);
			return rv.promise();
		}
		$(function(){
            //then(done,fail)
			hasWebP().then(function(){console.log(1)},function(){console.log(2)});
		})
````

* Deffered可以接受一个函数名参数

````
   function func(rv){
        //rv是Deferred对象
	　　　　var tasks = function(){
	　　　　　　alert("done！");
		   rv.resolve();
	　　　　};
	　　　　setTimeout(tasks,3000);
		return rv.promise();
	}
   $.Deferred(func)
　　.done(function(){ alert("success！"); })
　　.fail(function(){ alert("error！"); });
````

* always方法，执行完done,fail后执行

````
   $.Deferred(func)
　　.done(function(){ alert("success！"); })
　　.fail(function(){ alert("error！"); })
  .always(function(){console.log('end');})
````