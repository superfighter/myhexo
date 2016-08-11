---
title: console
date: 2016-08-11 13:01:27
tags: javascript
---

内容来源：[码农网](http://www.codeceo.com/article/9-console-command.html)
* 常用命令
````
 	<script type="text/javascript">
     	console.log('hello');
     	console.info('信息');
     	console.error('错误');
     	console.warn('警告');
 	</script>
````
* 信息分组
````
	<script type="text/javascript">
		console.group("第一组信息");
			console.log("第一组第一条:我的博客(http://www.ido321.com)");
			console.log("第一组第二条:CSDN(http://blog.csdn.net/u011043843)");
	　　console.groupEnd();
		console.group("第二组信息");
			console.log("第二组第一条");
			console.log("第二组第二条:欢迎你加入");
		console.groupEnd();
	</script>
````
<!-- more -->
* 查看对象信息
````
	var info = {
			blog:"http://superfighter.github.io",
			tel:110,
			message:"我的博客"
		};
 	console.dir(info);
````
* 显示节点内容
````
	var info = document.getElementById('info');//获取DOM元素
 	console.dirxml(info);
````
* 断言
````
	var result = 1;
    console.assert( result );
    var year = 2014;
    console.assert(year == 2018 );//控制台提示信息
````
* 查看调用栈
````
 	/*函数是如何被调用的，在其中加入console.trace()方法就可以了*/
　　function add(a,b){
	    console.trace();
		return a+b;
　　}
　　var x = add3(1,1);
 　 function add3(a,b){return add2(a,b);}
　　function add2(a,b){return add1(a,b);}
　　function add1(a,b){return add(a,b);}
````
* 计算运行时间
````
 　　console.time("控制台计时器一");
 　　for(var i=0;i<1000;i++){
 　　　　for(var j=0;j<1000;j++){}
 　　}
 　　console.timeEnd("控制台计时器一");//控制台披露时间：100ms
````
* 性能分析
````
	//每个浏览器的性能选项（性能分析器）夹里查看结果：chrome->profile; firefox->性能
 　　function All(){
　　　　     for(var i=0;i<10;i++){
             funcA(1000);
        }
　　　　    funcB(10000);
　　 }

 　　function funcA(count){
 　　　　for(var i=0;i<count;i++){}
 　　}

 　　function funcB(count){
 　　　　for(var i=0;i<count;i++){}
 　　}

 　　console.profile('性能分析器');
 　　All();
 　　console.profileEnd();
````
