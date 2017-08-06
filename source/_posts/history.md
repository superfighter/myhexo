---
title: html5-history
date: 2016-04-05 10:41:56
tags: html5
---
<a class="a" href="?id=a" title="想看美女 | 有道">我是美女，想知道我的名字就点我吧！</a>
<a class="a" href="?id=b" title="想看丑女 | 有道">我是丑女，想知道我的名字就点我吧！</a>
<strong style="display:none"></strong>
<!-- more -->
<script type="text/javascript" src="http://apps.bdimg.com/libs/jquery/1.9.1/jquery.js"></script>
<script type="text/javascript">
$(function(){
	var title = document.title;
	function setName(h){
		var search = /\?([^=]*)=(.*)/igm.exec(h);
		if(search && search[2]){
			$.get('/json/test.json',function(data){
				$.each(data,function(k,v){
					if(v.id == search[2]){
						$('strong').html('很高兴认识你，我是' + getName(v)).show();	
						document.title = getName(v) + '| 有道';
						return false;
					}
				});
			});
		} else {
			document.title = title;
			$('strong').hide();
		}
		
		function getName(d){
			return d.name;
		}
	}
	$('.a').bind('click',function(e){
		setName($(this).attr('href'));
		history.pushState(null, $(this).attr('title'), $(this).attr('href'));
		e.preventDefault();
	});

	if(history.pushState){
		window.addEventListener("popstate", function(){
			setName(location.href);
		})
		setName(location.href);
	}
})
</script>
