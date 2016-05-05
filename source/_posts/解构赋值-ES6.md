---
title: 解构赋值-ES6
date: 2016-04-29 15:22:24
tags:
	- es6
---
主要参考来源[阮一峰](http://es6.ruanyifeng.com/#docs/destructuring)

## 解构赋值 ##
### 默认解构赋值 ###
	
	let [x] = [1];
	//x => 1

### 对象的解构赋值 ###

	var {bar , foo} = {foo:'hello', bar:'world'};

<!-- more -->
`这里注意：{模式:变量}，上面可以理解为var {bar: bar, foo: foo}`

	// foo => hello, bar => world
	var {bar: zoo} = {bar: 'hello world'}
	// zoo => hello world
	// bar => undefined
	let foo;
	let {foo} = {foo:1}
	// => error
	var obj = {
		p:[
			"hello",
			{y: "world"}
		]
	}
	var {p: [x,{y}]} = obj;
	// x => hello
	// y => world
	var wtd = {
		name: 'wtd',
		say: 'my name is wtd'
	}
	let {name, say} = wtd;
	//say => my name is wtd

### 字符串的解构赋值 ###

	let [a, b ,c] = 'hello'
	//a => h
	//c => l
	let {length: len} = 'hello'
	// len => 5

`这里注意解构赋值时，如果等号右边是字符串，则会先转为类似数组的对象。`
### 数值和布尔值的解构赋值 ###

	var {toString:s} = true;
	//s => Boolean.prototype.toString

`这里注意解构赋值时，如果等号右边是数值和布尔值，则会先转为对象。`
### 函数参数的解构赋值 ###

	function add([x,y]){
		return x + y;
	}
	// add([1,2]) => 3
	[[1,2],[3,4]].map(([a,b])=>a+b)
	//[3,7]
	let skd = ([a = 2,b = 4]=[])=>{
	  a++;
	  b++;
	  return a+b;
	};
	// skd([]) => 8 
	// 传入{} 会报错 Invalid attempt to destructure non-iterable instance
	[1,undefined,3].map((x='yes')=>x);
	//[1,'yes',3]

`这里注意！！！！undefined 会触发默认值！！！！`

### 圆括号的使用注意点 ###
变脸声明语句中，不能带有圆括号
赋值声明语句中的非模式部分可以带有圆括号
### 解构赋值的用途 ###
* 交换变量的值
	
	[x,y] = [y,x]

* 从函数返回多个值

	function example(){
		return [1,2,3];
	}
	var [a,b,c] = example();

* 读取JSON值
* 函数参数定义
* 指定传入默认值
* 遍历MAP解构

	var m = new Map();
	m.set('first','first hello);
	for(let [key, value] of m){
		console.log(key, ' is ', value);//first is first hello
	}
