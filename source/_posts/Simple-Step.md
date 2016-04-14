---
title: Simple-Step
date: 2016-04-15 00:22:40
tags: [javascript,css3]
---
简单的分步例子，运用css3处理步骤切换~
<!-- more -->
```
	<!DOCTYPE html>
	<html>
	<head>
	  <meta charset="utf-8">
	  <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1, minimum-scale=1, user-scalable=no">
	  <meta name="format-detection" content="telephone=no,email=no,adress=no">
	  <title>简单分步</title>
	  <style>
	  * {padding: 0; margin: 0;}
	  .page{ height: 500px; background: blue; color: #FFF; float: left; position: relative;}
	  .page:nth-child(1){
	    background: black;
	  }
	  .page:nth-child(2){
	    background: red;
	  }
	  body{
	    overflow-x: hidden; 
	  }
	  </style>

	</head>
	<body>
	<div class="page">
	    <p class="j_goPage" data-page="1">1111111111111111111111111111111</p>
	</div>

	<div class="page">
	    <p class="j_goPage" data-page="2">2222222222222222222222222222222</p>
	</div>

	<div class="page">
	    <p class="j_goPage" data-page="0">3333333333333333333333333333333</p>
	</div>

	</body>
	</html>
	<script src="thsft/static/jquery/jquery.min.js"></script>
	<script src="thsft/static/seajs/sea.js"></script>
	<script src="thsft/static/seajs/config.js"></script>

	<script>
	seajs.use('module/moduleA',function(A){
	    // console.log(A);
	    var winWidth = window.innerWidth;
	    $('body').width($('.page').length * winWidth);
	    var pSize = $('.page').length;
	    var zIndex = 1;
	    $(function(){
	        $('.page').width(winWidth);
	        $('.j_goPage').each(function(i,v){
	            $(v).click(function(){
	                zIndex++;
	                // alert($(this).data('page'));
	                var cPage = $(this).parents('.page').index();
	                console.log(cPage);
	                var tPage = $(this).data('page');
	                console.log(tPage);
	                var cStyle = $('.page:eq('+cPage+')').attr('style');
	                var tStyle = $('.page:eq('+(tPage)+')').attr('style');
	                cStyle = cStyle.replace(/(.*)transform:\s?translateX\([^\)]*\);?(.*)/img,'$1$2');
	                tStyle = tStyle.replace(/(.*)transform:\s?translateX\([^\)]*\);?(.*)/img,'$1$2');
	                if(tPage < cPage){
	                    $('.page:eq('+cPage+')').attr('style',cStyle);
	                    $('.page:eq('+(tPage)+')').attr('style',tStyle + 'transform: translateX('+tPage * winWidth+'px)').css('zIndex', zIndex);
	                } else if(tPage > cPage) {
	                    $('.page:eq('+cPage+')').attr('style',cStyle + 'transform: translateX(-'+winWidth+'px)');
	                    $('.page:eq('+(tPage)+')').attr('style',tStyle + 'transform: translateX(-'+tPage * winWidth+'px)').css('zIndex', zIndex);
	                }
	            })
	        })
	    })
	});
	</script>
```