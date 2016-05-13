---
title: 'RegExp-ES6 '
date: 2016-05-10 14:48:31
tags:
	- RegExp
	- javascript
	- es6
---
* 新增u修饰符
* new RegExp(/x/i, 'gi') // 会覆盖前面定义的修饰符
* /^.$/.test('吉') //false
* /^.$/u.test('吉') //true
* /\u{3}/.test('uuu') //{}被解读为量词
* /\u{61}/u.test('a') //解读为Unicode
* 有些Unicode字符的编码不同，但是字型很相近，比如，\u004B与\u212A都是大写的K。
* y修饰符的作用与g修饰符类似，也是全局匹配，后一次匹配都从上一次匹配成功的下一个位置开始
* 可以重置lastIndex来改变匹配起始位置
````
	var sd = 'aaa_aa_a';
	var r1 = /a+/g;
	var r2 = /a+/y;

	console.log(r1.exec('aaa_a1a_a'))
	console.log(r1.exec('aaaaaa_')) //即使是匹配不同字符串，相同的正则仍然会保留上一次匹配后的起始位置
	r1.lastIndex=0;
	console.log(r1.exec('a_aaaa_aaaa'))
````
* split：aaa_aaa_aa 如果以a为分隔符，结果是['','','','_','','','_','',''] //('')a('')a('')a('_')a('')a('')a('_')a('')a('')
* y修饰符确保匹配必须从剩余的第一个位置开始
````
	const REGEX = /a/gy; //最后一个a因为不是出现下一次匹配的头部，所以不会被替换
	console.log('aaxa'.replace(REGEX, '-')) // '--xa'
````
* sticky // RegExp.sticky判断是否使用粘性修饰符
* source 返回正则匹配正文
* flags 返回正则修饰符
* 字符串必须转义，才能作为正则模式
````
	//可用于fiddler 匹配*请求中http:\/\/xxx.xxx.cn\/\?v=.*
	function escapeRegExp(str) {
	  return str.replace(/[\-\[\]\/\{\}\(\)\*\+\?\.\\\^\$\|]/g, '\\$&');
	}

	let str = '/path/to/resource.html?search=query';
	escapeRegExp(str)
	// "\/path\/to\/resource\.html\?search=query"
````
* 后行断言，从右往左执行表达式
````
	/(?<=(\d+)(\d+))$/.exec('1053') // ["", "1", "053"]
	/^(\d+)(\d+)$/.exec('1053') // ["1053", "105", "3"]
````