---
title: Iterator
date: 2016-07-20 16:39:38
tags:
	- Iterator
	- es6
---
* javscript集合对象：Array,Object,Set,Map
* 遍历器是一种接口，为各种不同的数据结构提供统一的访问机制。任何数据结构只要部署Iterator接口，就可以完成遍历操作（即依次处理该数据结构的所有成员）
* Iterator的作用有三个：
	* 为各种数据结构提供一个统一的、简便的访问接口
	* 数据结构的成员能够按某种次序排列
	* ES6创造了一种新的遍历命令for...of循环，Iterator接口主要供for...of消费。
* Iterator的遍历过程：
	* 创建一个指针对象，指向当前数据结构的起始位置。本质是一个指针对象。
	* 第一次调用指针对象的next方法，可以将指针指向数据结构的第一个成员。
	* 第二次调用指针对象的next方法，指针就指向数据结构的第二个成员。
	* 不断调用指针对象的Next方法，直到它指向数据结构的结束位置。
	* 每次调用next方法，都会返回数据结构的当前成员的信息。具体来说，就是返回一个包含value和done两个属性的对象。其中，value属性是当前成员的值，done属性是一个布尔值，表示遍历是否结束。
* 自定义实现一个拥有Iterator接口的对象，必须要在Symbol.iterator的属性上部署遍历器生成方法(原型链上也可以)
````
class RangeIterator{
	constructor(start, stop){
		this.value = start;
		this.stop = stop;
	}
	[Symbol.iterator](){ return this;}
	next(){
		var value = this.value;
		if(value < this.stop){
			this.value++;
			return {done:false,value: value};
		} else {
			return {done: true}
		}
	}
}
//原型链
function Obj(value){
	this.value = value;
	this.next = null;
}
Obj.prototype[Symbol.iterator] = function(){
	var iterator = {
		next : next
	}
	var current = this;
	function next(){
		if(current){
			var value = current.value;
			current = current.next;
			return {
				done: false,
				value: value
			}
		} else {
			return {done: true}
		}
	}
	return iterator;
}

var one = new Obj(1);
var two = new Obj(2);
var three = new Obj(3);

one.next = two;
two.next = three;
for(var i of one){console.log(i)}//1 2 3
````
