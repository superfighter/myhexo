---
title: 'console%'
date: 2017-06-14 15:25:28
tags: 
	- javascript
	- console
---
* 大约一年前介绍过一篇`console`API的文章[这里](/2016/08/11/console/)
* 今天看到相关占位符的API，也记录下，供以后参考，内容直白不转弯，上代码；
* `%c` 样式

	```
		console.log('%cblue','color: blue')
	```
* `%d` 类型转换成整型，不四舍五入

	```
		console.log('%dblue','3') // NaNblue
	```

	```
		console.log('%dblue', 3) // 3blue
	```
* `%f` 类型转成浮点型

	```
		console.log('%fblue','3.12') // NaNblue
	```

	```
		console.log('%fblue', 3.33) // 3.33blue
	```
* `%o` 类型转成对象

	```
		console.log('%oblue','3.12') // "3.12"blue
	```

	```
		console.log('%oblue', [3.33]) // Arrayblue
	```
* `%s` 类型转成字符串

	```
		console.log('%sblue','3.12') // 3.12blue
	```

	```
		console.log('%sblue', [3.33]) // Array(1)blue
	```