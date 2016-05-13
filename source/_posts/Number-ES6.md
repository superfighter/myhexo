---
title: Number-ES6
date: 2016-05-12 10:02:20
tags:
	- es6
	- javascript
---
* 二进制和八进制表示法
````
	// 分别用前缀0b、0B和0o、0O表示
	0b111110111 === 503
	0o767 === 503 //八进制
````
* Number转为十进制,Number('0o10') == 8
* 极小误差
````
	function withinErrorMargin (left, right) {
  		return Math.abs(left - right) < Number.EPSILON
	}
	console.log(withinErrorMargin(0.1 + 0.2, 0.3))
````
* JavaScript能够准确表示的整数范围在-2^53到2^53之间
* Math对象扩展
	* Math.trunc()//去除小数部分
	* Math.sign()//判断正数、负数、零
	* Math.cbrt()//返回立方根
	* Math.clz32()//补零
	* Math.imul()//返回两个数以32位带符号整数形式相乘结果
	* Math.fround()// new Float32Array()
	* Math.hypot()//返回所有参数平方和的平方根
	* 对数方法
	* 三角函数
		* Math.sinh()//双曲正弦
		* Math.cosh()//双曲余弦
		* Math.tanh(x) 返回x的双曲正切（hyperbolic tangent）
		* Math.asinh(x) 返回x的反双曲正弦（inverse hyperbolic sine）
		* Math.acosh(x) 返回x的反双曲余弦（inverse hyperbolic cosine）
		* Math.atanh(x) 返回x的反双曲正切（inverse hyperbolic tangent）