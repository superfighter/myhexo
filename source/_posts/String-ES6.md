---
title: String-ES6
date: 2016-05-06 14:54:03
tags:
	- es6
	- javascript
---
* string.codePointAt()能处理4个字节
* string.charCodeAt()处理2个字节
````
	// 是否由四个字节组成
	function is32Bit(c){
		return c.codePointAt(0) > 0xFFFF;
	}
````
* string.fromCharCode(0xFFFF)
* string.fromCodePoint(0x20BB7)
* 如果String.fromCharCode方法有多个参数，则它们会被合并成一个字符串返回。String.fromCodePoint(0x78, 0x1f680, 0x79) === "x\uD83D\uDE80y" // true
* fromCodePoint 方法定义在String对象上，codePointAt方法定义在字符串实例对象上
* for(let xx of string) 可以识别大于0xFFFF的码点
* string.at()和string.charAt()
* string.includes()
* string.endsWith(sting1,index)
* string.startsWith()
* string.padStart(length,string1) //length是拼接string1后总长度，超出部分截出
* string.padEnd(length,string1)
### templete ###
* ``反引号成行成段转义输出
* let man = 6
* $('#J_main').append(`<b>${man}</b>`) // <b>6</b>
* ${内部执行的是JavaScript代码}
* ${'hello' + ' world'}
* console.log(`${man}`+`${man}`);
* console.log(`${man + man}`);
* let xx = (x)=>{return x;}
* console.log(`${xx(21)}`)
* let lo = `(function fk(x){ return x})` // 要用括号把函数体包裹起来
* let fuc = eval(lo)
* console.log(fuc(223))
* string.raw //转义\
* 简单模板
````
	var template = `
	<ul>
	  <% for(var i=0; i < data.supplies.length; i++) {%>
	    <li><%= data.supplies[i] %></li>
	  <% } %>
	</ul>
	`;
	function compile(template){
	  var evalExpr = /<%=(.+?)%>/g;
	  var expr = /<%([\s\S]+?)%>/g;

	  template = template
	    .replace(evalExpr, '`); \n  echo( $1 ); \n  echo(`')
	    .replace(expr, '`); \n $1 \n  echo(`');

	  template = 'echo(`' + template + '`);';

	  // console.log(template);
	  var script =
	  `(function partse(data){
	    var output = "";

	    function echo(html){
	      output += html;
	    }
	    ${ template }

	    return output;
	  })`;

	  return script;
	}
	
	var parse = eval(compile(template));
	console.log(parse({ supplies: [ "broom", "mop", "cleaner" ] }));
````
* 标签模板
````
	tag`Hello ${main}`
````