---
title: string.repeat
date: 2016-03-31 16:57:13
tags: javascript
---

* 主要参考[MDN](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/repeat)

* 简单介绍
	
		'abc'.repeat(-1);   // RangeError
		'abc'.repeat(0);    // ''
		'abc'.repeat(1);    // 'abc'
		'abc'.repeat(2);    // 'abcabc'
		'abc'.repeat(3.5);  // 'abcabcabc' (count will be converted to integer)
		'abc'.repeat(1/0);  // RangeError
		
		({ toString: () => 'abc', repeat: String.prototype.repeat }).repeat(2);
		// 'abcabc' (repeat() is a generic method)