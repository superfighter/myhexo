---
title: callee-caller
date: 2016-06-20 09:38:44
tags:
	- function
	- javascript
---

### callee ###
* callee是arguments的属性成员，返回正被执行的 Function 对象，也就是所指定的 Function 对象的正文
````
	function calleeLengthDemo(arg1, arg2) { 
		alert(arguments.callee.toString()); 
		if (arguments.length == arguments.callee.length) { 
			window.alert("验证形参和实参长度正确！"); 
			return; 
		} else { 
			alert("实参长度：" + arguments.length); 
			alert("形参长度： " + arguments.callee.length); 
		} 
	} 
	calleeLengthDemo(1); //实参长度：1，形参长度：2
	//=============================================//
	//应用于匿名函数调用自身场景
	var fn=(function(n){ 
	if(n>0) return n+arguments.callee(n-1); 
		return 0; 
	})(10); 
	alert(fn) 
````
### caller ###
* functionName.caller 返回调用者
````
	function caller() { 
		if (caller.caller) { 
			alert(caller.caller.toString()); 
		} else { 
			alert("函数直接执行"); 
		} 
	} 
	function handleCaller() { 
		caller(); 
	} 
	handleCaller(); //handleCaller
	caller(); // 函数直接执行
````