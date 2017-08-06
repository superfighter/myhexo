---
title: Array-ES6
date: 2016-05-12 11:37:00
tags:
	- es6
	- javascript
---
* Array.from(obj, func, this)//类似Array和可Iterator的对象转为Array, func类似于map对每个元素进行处理，将处理后的值放入返回的数组
* [].slice.call({'0':'1',length:1})// [1]
* ... 扩展运算符 Symbol.iterator
* 能正确处理Unicode，可以避免JavaScript将大于\uFFFF的Unicode字符，算作两个字符的bug
````
	function countSymbols(string) {
	  return Array.from(string).length;
	}
````
* Array.of('3') ,构造数组 new Array('3')
* Array.prototype.copyWithin(target, start = 0, end = this.length) //在数组内部进行替换元素, 在index为target的起始位置上，从index为start的元素开始复制，截到第end元素为止，数组总长度不变
````
	let popo = Array.of(3,3,4,5,12);
	console.log(popo.copyWithin(0,2,4)); // [4,5,4,5,12]
	console.log(popo.copyWithin(1,2,4)); // [3,4,5,5,12]
	console.log(popo.copyWithin(1,2,1)); // [3,3,4,5,12]返回元素组
	console.log(popo.copyWithin(1,2,3)); // [3,4,4,5,12]
	console.log(popo.copyWithin(1,2,-1));// [3,4,5,5,12] 负数从末尾算起
	console.log([1, 2, 3, 4, 5].copyWithin(0, -3, -2)); // [3,2,3,4,5] -3代表从末尾索引为-1开始算，-2代表末尾索引为0开始算
````
* Array.find
````
	//当前值，当前索引，当前数组
	[1, 5, 10, 15].find(function(value, index, arr) {
	  return value > 9;
	},this) // 10
````
* Array.findIndex
````
	[NaN].findIndex(y => Object.is(NaN, y))
````
* Array.fill填充一个数组
````
	Array.fill(string,start,end) //从start开始到end之前结束
````
* Array.entries(),Array.keys(),Array.values()
````
	let letter = ['a', 'b', 'c'];
	let entries = letter.entries();
	console.log(entries.next().value); // [0, 'a']
	console.log(entries.next().value); // [1, 'b']
	console.log(entries.next().value); // [2, 'c']
````
* Array.prototype.includes()
`
	Map结构的has方法，是用来查找键名的，比如Map.prototype.has(key)、WeakMap.prototype.has(key)、Reflect.has(target, propertyKey)。
	Set结构的has方法，是用来查找值的，比如Set.prototype.has(value)、WeakSet.prototype.has(value)。
`
* ES5数组对空位的处理
	* forEach(), filter(), every() 和some()都会跳过空位。
	* map()会跳过空位，但会保留这个值
	* join()和toString()会将空位视为undefined，而undefined和null会被处理成空字符串。
````
	// forEach方法
	[,'a'].forEach((x,i) => log(i)); // 1

	// filter方法
	['a',,'b'].filter(x => true) // ['a','b']

	// every方法
	[,'a'].every(x => x==='a') // true

	// some方法
	[,'a'].some(x => x !== 'a') // false

	// map方法
	[,'a'].map(x => 1) // [,1]

	// join方法
	[,'a',undefined,null].join('#') // "#a##"

	// toString方法
	[,'a',undefined,null].toString() // ",a,,"
````
* ES6则是明确将空位转为undefined
`Invalid attempt to destructure non-iterable instance`