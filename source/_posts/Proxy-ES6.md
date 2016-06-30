---
title: Proxy-ES6
date: 2016-06-30 14:10:26
tags:
	- proxy
	- es6
---
* Proxy用于修改某些操作的默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。
Proxy可以理解成，在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

````
//ES6生成Proxy实例
var proxy = new Proxy(target, handler);

var obj = new Proxy({}, {
  get: function (target, key, receiver) {
    console.log(`getting ${key}!`);
    return Reflect.get(target, key, receiver);
  },
  set: function (target, key, value, receiver) {
    console.log(`setting ${key}!`);
    return Reflect.set(target, key, value, receiver);
  }
});
````
* 要使得Proxy起作用，必须针对Proxy实例（上例是proxy对象）进行操作，而不是针对目标对象（上例是空对象）进行操作;如果handler没有设置任何拦截，那就等同于直接通向原对象。
````
var target = {};
var handler = {};
var proxy = new Proxy(target, handler);
proxy.a = 'b';
target.a // "b"
//技巧：将Proxy对象，设置到object.proxy属性，从而可以在object对象上调用
var object = { proxy: new Proxy(target, handler) };
//Proxy实例也可以作为其他对象的原型对象
var proxy = new Proxy({}, {
  get: function(target, property) {
    return 35;
  }
});

let obj = Object.create(proxy);
obj.time // 35

//可以进行多操作拦截
var handler = {
  get: function(target, name) {
    if (name === 'prototype') {
      return Object.prototype;
    }
    return 'Hello, ' + name;
  },

  apply: function(target, thisBinding, args) {
    return args[0];
  },

  construct: function(target, args) {
    return {value: args[1]};
  }
};

var fproxy = new Proxy(function(x, y) {
  return x + y;
}, handler);
fproxy(2,20);//apply ==> 2
new fproxy(1,11);//construct ==> {value: 11}
fproxy.hello;//get ==> Hello, hello
````
* Proxy支持的拦截操作如下
	* get(target, propKey, receiver)拦截对象属性的读取，比如proxy.foo和proxy['foo']，返回类型不限。最后一个参数receiver可选，当target对象设置了propKey属性的get函数时，receiver对象会绑定get函数的this对象。
	* set(target, propKey, value, receiver)拦截对象属性的设置，比如proxy.foo = v或proxy['foo'] = v，返回一个布尔值。
	* has(target, propKey)拦截propKey in proxy的操作，以及对象的hasOwnProperty方法，返回一个布尔值。
	* deleteProperty(target, propKey)拦截delete proxy[propKey]的操作，返回一个布尔值。
	* ownKeys(target)拦截Object.getOwnPropertyNames(proxy)、Object.getOwnPropertySymbols(proxy)、Object.keys(proxy)，返回一个数组。该方法返回对象所有自身的属性，而Object.keys()仅返回对象可遍历的属性。
	* getOwnPropertyDescriptor(target, propKey)拦截Object.getOwnPropertyDescriptor(proxy, propKey)，返回属性的描述对象
	* defineProperty(target, propKey, propDesc)拦截Object.defineProperty(proxy, propKey, propDesc）、Object.defineProperties(proxy, propDescs)，返回一个布尔值。
	* preventExtensions(target)拦截Object.preventExtensions(proxy)，返回一个布尔值。
	* isExtensible(target)拦截Object.isExtensible(proxy)，返回一个布尔值。
	* setPrototypeOf(target, proto)拦截Object.setPrototypeOf(proxy, proto)，返回一个布尔值。
	* apply(target, object, args)拦截Proxy实例作为函数调用的操作，比如proxy(...args)、proxy.call(object, ...args)、proxy.apply(...)。
	* construct(target, args)拦截Proxy实例作为构造函数调用的操作，比如new proxy(...args)。

* 各拦截操作实战用途如下
	* get 可以在读取不存在的属性时抛出一个错误，而不是仅仅返回无意义的undefined；可以在返回之前做逻辑处理，比如实现数组负数索引取值；可以创建链式调用。
	* set 可以用来实施更新DOM
	* has 可以对in运算符做特殊处理，如不让带大写A的属性被in发现；has方法拦截的是HasProperty操作，而不是HasOwnProperty操作，即has方法不判断一个属性是对象自身的属性，还是继承的属性。for...in操作内部也是用到HasProperty操作，所以has方法在for...in循环时也会生效。
	* construct方法返回的必须是一个对象，否则会报错。
	````
		//在返回真正的对象前可以加以处理，但返回值必须是对象
		var p = new Proxy(function() {}, {
	  	construct: function(target, args) {
		    console.log('called: ' + args.join(', '));
		    return { value: args[0] * 30 };
		  }
		});

		new p(10).value
		// "called: 10"
		// 300
	````
	* getPrototypeOf用来拦截object.getPrototypeOf()运算符，以及Reflect.getPrototypeOf,instanceof运算符，Object.prototype.isPrototypeOf
* Proxy.revocable(),Proxy.revocable方法返回一个对象，该对象的proxy属性是Proxy实例，revoke属性是一个函数，可以取消Proxy实例。上面代码中，当执行revoke函数之后，再访问Proxy实例，就会抛出一个错误。
````
let target = {};
let handler = {};

let {proxy, revoke} = Proxy.revocable(target, handler);
proxy.foo = 123;
proxy.foo //123
revoke();
proxy.foo // TypeError : Revoke
````