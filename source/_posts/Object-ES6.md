---
title: 'Object-ES6 '
date: 2016-05-22 09:55:02
tags:
	- es6
	- javascript
---

* 属性简写：允许只写对象属性名，不写属性值
````
let foo = 'hello'
let bar = {foo} // bar.foo = 'hello'
function x(my,other){ return {my,other}}
let o = x(1,2)// o.my = 1 , o.other = 2
````
* 方法简写
````
let obj = {
	method(){
		return "hello~"
	}
}
console.log(obj.method()) // hello~
````
* 方括号定义对象变量
````
 	//ES5
 	var bar = {"foo":"hello"}
 	//ES6
 	let f = "foo"
 	let bar = {[f]:'hello'} // bar.foo = hello
````
* Object.is
````
Object.defineProperty(Object, 'is', {
  value: function(x, y) {
    if (x === y) {
      // 针对+0 不等于 -0的情况
      return x !== 0 || 1 / x === 1 / y;
    }
    // 针对NaN的情况
    return x !== x && y !== y;
  },
  configurable: true,
  enumerable: false,
  writable: true
});
````
* Object.assin 合并可枚举对象
	* 为对象添加属性
	* 为对象添加方法
	* 克隆对象
	* 合并多个对象
	* 为属性指定默认值
* Object.getOwnPropertyDescriptor 属性的可枚举性
	* ES5会有三个操作忽略enumerable为false的属性
		* for...in循环：只遍历对象自身的和继承的可枚举的属性
		* Object.keys():返回对象自身的所有可枚举的属性键名
		* JSON.stringify:只串行化对象自身的可枚举的属性
	* ES6新增会忽略enumerable为false的属性
		* Object.assign():只拷贝对象自身可枚举的属性
		* Reflect.enumerate():返回所有for...in循环会遍历的属性