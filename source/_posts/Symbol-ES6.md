---
title: Symbol-ES6
date: 2016-06-05 14:54:28
tags:
	- string
	- symbol
	- es6
---

* symbol 可以转换成字符串类型、布尔类型，不能转换成数值。
```
	var s = Symbol('a')
	s.toString()// Symbol(a)
	!s // false
	Number(s) // TypeError
```
* symbol定义时不能用.运算符
```
	var mysymbol = Symbol()
	var a = {}
	a.mysymbol = 'hello'
	//因为点运算符后面总是字符串，所以不会读取mysymbol作为标识名所指代的那个值，导致a的属性名实际上是一个字符串，而不是一个Symbol值
	a[mysymbol] //undefined
	a['mysymbol'] // hello
```
* symbol.for('string') 和 symbol('string') 的区别是会在全局环境中搜索 string 是否已经存在，如果没有存在则新建一个值
```
	var s1 = Symbol.for('a')
	var s2 = Symbol.for('a')

	s1 == s2 //true

	var s3 = Symbol('c')
	var s4 = Symbol('d')

	s3 == s4 //false
```
* symbol.keyfor('string') 返回一个已登记的Symbol值，在全局环境中起作用，可以在不同iframe和service worker中起作用
```
	var s1 = Symbol.for('cc')
	symbol.keyfor(s1) // cc
	symbol.keyfor(s2) // undefined
```
* 内置symbol
	* Symbol.hasInstance,`foo instance Foo`内部是`Foo[Symbol.hasInstance](foo)`
	````
		class MyClass {
		  [Symbol.hasInstance](foo) {
		    return foo instanceof Array;
		  }
		}

		[1, 2, 3] instanceof MyClass() // true
	````
	* Symbol.isConcatSpreadable 对象是否可以展开
	````
		//数组对象默认展开
		let arr1 = ['c', 'd'];
		['a', 'b'].concat(arr1, 'e') // ['a', 'b', 'c', 'd', 'e']

		let arr2 = ['c', 'd'];
		arr2[Symbol.isConcatSpreadable] = false;
		['a', 'b'].concat(arr2, 'e') // ['a', 'b', ['c','d'], 'e']
		//字面量对象默认收起
		let obj = {length: 2, 0: 'c', 1: 'd'};
		['a', 'b'].concat(obj, 'e') // ['a', 'b', obj, 'e']

		obj[Symbol.isConcatSpreadable] = true;
		['a', 'b'].concat(obj, 'e') // ['a', 'b', 'c', 'd', 'e']
	````
	* Symbol.species 指向一个方法,该对象做为构造函数创造实例时，会调用这个方法。
	* Symbol.match 指向一个函数，当执行str.match(myObject)时，如果该属性存在，会调用它，返回该方法的返回值
	````
		String.prototype.match(regexp)
		// 等同于
		regexp[Symbol.match](this)

		class MyMatcher {
		  [Symbol.match](string) {
		    return 'hello world'.indexOf(string);
		  }
		}

		'e'.match(new MyMatcher()) // 1
	````
	* Symbol.replace 指向一个方法,当该对象被String.prototype.replace调用时，会返回该方法的返回值
	* Symbol.search 指向一个方法，当该对象被String.prototype.search调用时，会返回该方法的返回值
	* Symbol.split 指向一个方法，当该对象被String.prototye.split调用时，会返回该方法的返回值
	* Symbol.iterator 指向该对象默认遍历器方法.对象进行for...of循环时，会调用Symbol.iterator方法，返回该对象的默认遍历器
	* Symbol.toPrimitive
	````
		let obj = {
		  	[Symbol.toPrimitive](hint) {
			    switch (hint) {
			      case 'number':
			        return 123;
			      case 'string':
			        return 'str';
			      case 'default':
			      	// 既可以转成字符串也可以转成数字时为default状态
			        return 'default';
			      default:
			        throw new Error();
		     	}
		   	}
		};

		2 * obj // 246
		3 + obj // '3default'
		obj === 'default' // true
		String(obj) // 'str'
	````
	* Symbol.toStringTag 被Object.prototype.toString调用时触发
	````
		class Collection {
		  get [Symbol.toStringTag]() {
		    return 'x2x3x';
		  }
		}
		var x = new Collection();
		console.log(Object.prototype.toString.call(x)) // "[object x2x3x]"
	````
	* Symbol.unscopables 指向一个对象，设置哪些属性可被with环境排除
	````
		// 没有unscopables时
		class MyClass {
		  foo() { return 1; }
		}

		var foo = function () { return 2; };

		with (MyClass.prototype) {
		  foo(); // 1
		}

		// 有unscopables时
		class MyClass {
		  foo() { return 1; }
		  get [Symbol.unscopables]() {
		    return { foo: true };
		  }
		}

		var foo = function () { return 2; };

		with (MyClass.prototype) {
		  foo(); // 2
		}
	````