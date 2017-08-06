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
* :: 返回的是原对象
````
	//上下文::执行方法
	foo::bar;
	// 等同于
	bar.bind(foo);

	foo::bar(...arguments);
	// 等同于
	bar.apply(foo, arguments);

	const hasOwnProperty = Object.prototype.hasOwnProperty;
	function hasOwn(obj, key) {
	  return obj::hasOwnProperty(key);
	}
````
* 尾调=>调用栈
* stack overflow 栈溢出
* 尾递归
````
	//复杂度O(n)
	function factorial(n) {
	  if (n === 1) return 1;
	  return n * factorial(n - 1);
	}
	factorial(5) // 120
	//5 * factorial(4)
	//5 * (4 * factorial(3))
	//5 * (4 * (3 * factorial(2)))
	//5 * (4 * (3 * (2 * factorial(1))))
	//5 * 4 * 3 * 2 * 1 
	//复杂度O(1)
	function factorial(n, total){
		if(n === 1) return total;
		return factorial(n-1, n * total);
	}
	factorial(5,1) // 120
	// 只保留一个调用帧
	// factorial(5,1)
	// factorial(4,5)
	// factorial(3,20)
	// factorial(2,60)
	// factorial(1,120)
````
`
尾递归的实现，往往需要改写递归函数，确保最后一步只调用自身。做到这一点的方法，就是把所有用到的内部变量改写成函数的参数。比如上面的例子，阶乘函数 factorial 需要用到一个中间变量 total ，那就把这个中间变量改写成函数的参数。
`
* 闭包记忆 [简书资料](http://www.jianshu.com/p/a2dfa59e70d7)
````
//cache为function内局部变量，cache被匿名函数引用，匿名函数上下文为全局变量window
//memorize执行完后，cache理应被回收，但是在被匿名函数引用的情况下无法回收
function memorize(sets, f) {
    var cache = {}; 
    return function (x) { 
        console.log('cache: %j', cache);
        return x in cache
               ? cache[x]
               : cache[x] = f(sets[x]);
    }
}
var g = memorize([1000, 2000, 3000], function (x) { return x * x; });
````
* console.log('%c Hello~how are you','font-size:28px; font-family:Microsoft YAHEI;color: blue; font-weight: bold;')

/*Object*/
* Object.assign 
* //除了字符串会以数组形式拷贝入目标对象，数值、布尔、undefined、null都不起效果（这是因为只有字符串的包装对象，会产生可枚举属性）
* //首参数不能是Null、Undefined ：Cannot convert undefined or null to object
