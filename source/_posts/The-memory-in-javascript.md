---
title: The memory in javascript
date: 2017-09-03 09:20:00
tags:
	- javascript
	- memory
---
## 前言
----
最近在和新同事的沟通中被问到了javascript是如何管理内存的，以前虽有了解过，但没有系统的记忆，所以答的不理想，乘此机会在这里尽可能整理记忆下。
### 内存
#### 什么是内存
定义内存就是暂时存储程序和数据的地方。程序语言中低级语言一般都有低级的内存管理接口供开发者有选择性的释放内存，如C语言。高级语言则在创建变量时选择性的分配内存，然后在不使用的时候释放自动释放，这种『垃圾回收』机制并不意味着在高级语言开发中可以不顾内存管理因素肆意滥用。
#### 生命周期
大体了解一个框架可以从生命周期入手，如React，Vue。其实了解任何已被定义的事物都可以如此。程序语言中的内存生命周期大体一致：
* 分配所需要的内存
* （读，写）内存
* 不使用时释放/归还内存
在所有语言中第一步和第二步都很清晰，但是在javascript中，第三步是隐藏的、不透明的，弄清楚第三步是如何执行的体现了差异性。

#### 内存策略
javascript中分堆内存和栈内存。
* 堆内存
堆内存用于存放new创建的对象和数组，因为javascript无法确定声明的函数或者数组对象的具体内存大小，所以无法存放在栈内。它是由虚拟机自动垃圾回收来管理。通常会在栈中定义一个特殊变量，这个变量的取值等于数组或者对象在堆内存中的地址，这就创建了引用变量。数组和对象本身在堆中的分配即时程序在运行到new产生数组和对象的代码块之外，也不会被释放，数组和对象在没有引用变量指向它的时候，才变成垃圾，但只是不能被使用，仍然占用着堆内存，在随后的一个不确定时间被回收。
````
	var o = {
	  a: 1,
	  b: null
	}; // 给对象及其包含的值分配内存

	// 给数组及其包含的值分配内存（就像对象一样）
	var a = [1, null, "a"]; 

	function f(a){
	  return a + 2;
	} // 给函数（可调用的对象）分配内存

	// 函数表达式也能分配一个对象
	someElement.addEventListener('click', function(){
	  someElement.style.backgroundColor = 'blue';
	}, false);
````
* 栈内存
基本类型的变量和对象都是在栈内存中分配。当在一个代码块中定义一个变量时，系统就为这个变量分配内存空间，在当前作用域不包含此变量时，释放栈内存空间。
````
	var n = 1; // 给数值变量分配内存
	var s = "a"; // 给字符串分配内存
````
#### 读写内存
<!-- more -->
读写内存实际上是使用或改变变量的值，可能是改变变量的当前值，可能是改变变量的一个属性值，可能是传入函数的参数。
#### 释放内存
前面也提到，释放内存就是在内存不被使用的情况下进行的。但就是最难的地方『被分配的内存确实不在被需要了』。javascript中如何确定一个内存不再被需要？那就是引用，一个对象没有被其他任何对象引用（零引用）,对象将会被回收。
* 显式引用
	对象属性的引用。
* 隐式引用
	对象原型的引用。

#### 实例
* 引用计数垃圾收集
````
	// 对象值
	var o = { 
	  a: {
	    b:2
	  }
	}; 
	// 两个对象被创建，一个『{b:2}』作为另一个的属性被引用，另一个『{a:{b:2}}对象值』被分配给变量o
	// 很显然，没有一个可以被垃圾收集


	var o2 = o; // o2变量是第二个对『对象值』的引用

	o = 1;      // 现在，『对象值』的原始引用o被o2替换了

	var oa = o2.a; // 引用『对象值』的a属性
	// 现在，『对象值』有两个引用了，一个是o2，一个是oa

	o2 = "yo"; // 最初的『对象值』现在已经是零引用了
	           // 他可以被垃圾回收了
	           // 然而它的属性a的对象还在被oa引用，所以还不能回收

	oa = null; // a属性的那个『对象值』现在也是零引用了
	           // 它可以被垃圾回收了
````
上面是来自MDN的简单引用计数案例，另外注意循环引用的处理。
````
	// 常见的循环引用
	// DOM对象
	var div;
	window.onload = function(){
	  div = document.getElementById("myDivElement");
	  div.circularReference = div; // circularReference 属性引用了DOM元素本身
	  div.lotsOfData = new Array(10000).join("*");
	};
	// 事件绑定
	function doClick() {}
	element.attachEvent("onclick", doClick); // 要记得页面卸载时detach
	// 对象属性
	var MyObject = {};
	document.getElementByIdx_x('myDiv').myProp = MyObject;
	// 普通对象
	function f(){
	  var o = {};
	  var o2 = {};
	  o.a = o2; // o 引用 o2
	  o2.a = o; // o2 引用 o

	  return "...";
	}

	f();
	// 闭包
	var val = 'hello world';
	function foo() {
		return function() {
			return val;
			};
	}
	global.bar = foo();
	// 全局变量赋值
	function g() {
		var c = 1;
		window.a = c;
	}
	g();
````
针对循环引用，所有浏览器开发商都采用了『标记-清除算法』。这个算法假定设置一个根对象，垃圾回收器定期的从根出发，找所有从根开始引用的对象，然后再找这些对象的引用对象，最终垃圾回收器可以找到可以获得的对象和无法获得的对象，最终回收不可获得的对象。在javascript中，window对象就是root对象，因此一旦div和其事件处理无法从window中获取到，他们将被垃圾回收器回收。
#### 小结
* 内存是在不断变化的，当变量或对象的值变动后，会重新申请新的内存，但是有上限值。V8的内存上限是732MB/1464MB。
* 老外总结的绕过内存泄露的技巧，供参考。
````
	window.onload=function outerFunction(){
		var anotherObj = function innerFunction(){
            alert("Hi! I have avoided the leak");
     	};
     	(function anotherInnerFunction(){
	        var obj =  document.getElementById("element");
	        obj.onclick=anotherObj })();
    };

    window.onload=function outerFunction(){
    	var obj = document.getElementById("element");
	    obj.onclick=function innerFunction(){
	        alert("Hi! I have avoided the leak");
	    };
    	obj.bigString=new Array(1000).join(new Array(2000).join("XXXXX"));
    	obj = null; //This breaks the circular reference
    };

    window.onload=function(){
	    var obj = document.getElementById("element");
	    obj.onclick = doesNotLeak;
	}
	function doesNotLeak(){
	    alert("Hi! I have avoided the leak");
	}
````
