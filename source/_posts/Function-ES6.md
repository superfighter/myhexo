---
title: Function-ES6
date: 2016-05-13 11:16:55
tags:
	- es6
	- javascript
---
* 参数默认值
* arguments
	* length不包括rese参数(...values)
	* length不包括赋默认值参数(values=1)
````
console.log((function A(a,dm,s,...v){}).length) // 3
console.log((function A(a,dm,s,v=1){}).length) // 3
````
* 作用域
* 必输参数
````
function throwMiss(){
  throw new Error('arguments error')
}
function gg(x=throwMiss()){ console.log(x)}
gg(2) // 2
gg() // arguments error
````
* rest参数
````
	function add(...values){
		//...values == [2,3,4,5] 从参数末尾索引开始计算，截止到最后一个有效参数索引为止
		let sum = 0;
		for (var val of values){
			sum += val;
		}
		return sum;
	}
	add(2,3,4,5);
````
* 扩展运算符...
````
	console.log(...[1,5,6]) // 1 5 6
	console.log(...[1,[5,6]]) // 1 [5,6]
	console.log(...[1,...[5,6]]) // 1 5 6
	console.log(...{'0':'12','1':'34',length:2}) // 12 34
* 凡是涉及到操作32位Unicode字符的函数
````
	'x\uD83D\uDE80y'.length // 4 wrong
	[...'x\uD83D\uDE80y'].length // 3 right
	function length(str){ return [...str].length}
````
* map, generator function可以使用...
* 如果对没有iterator接口的对象，使用扩展运算符，将会报错
* 箭头函数
	*函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。
	*不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。
	*不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。
	*不可以使用yield命令，因此箭头函数不能用作Generator函数。
````
	function Timer () {
	  this.s1 = 14;
	  this.s2 = 4;
	  // 箭头函数
	  // this指向定义时所在作用域，即Timer
	  setInterval(() => this.s1++, 1000);
	  // 普通函数
	  // this指向执行时作用域，即window
	  setInterval(function () {
	    this.s2++;
	  }, 1000);
	}
````
	* 箭头函数可以让this指向固定化
````
	var handler = {
	  id: '123456',

	  init: function() {
	  	//this指向handler，因为=>没有自己的this(this == undefined)，所以指向最近的外部this
	    document.addEventListener('click',
	      event => this.doSomething(event.type), false);
        // var self = this;
	    // document.addEventListener('click',function(){
	    //   self.doSomething(event.type)
	    // },false)
	  },

	  doSomething: function(type) {
	    console.log('Handling ' + type  + ' for ' + this.id);
	  }
	};
````
	* call\apply\bind无效，会忽略
````
	(function() {
	  return [
	    (() => this.x).bind({ x: 'inner' })()
	  ]
	}).call({ x: 'outer' });
	// function () {
	//   var _this = this;
	//   return [function () {
	//     return _this.x;
	//   }.bind({ x: 'inner' })()];
	// }.call({ x: 'outer' })
````