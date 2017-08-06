---
title: node-debugger
date: 2016-10-18 21:06:17
tags: node
---
* javascript运行在宿主环境中，浏览器、node都是宿主。
* javascript中常用debugger断点查看脚本运行状态，在浏览器环境下(这里以Chrome为例，其他高级浏览器雷同)按F12键可以看到调试工具栏，选择`Sources`标签栏，左侧是页面引入的资源列表，中间是对应源码，右侧就是调试界面，右侧最顶部的按钮依次是：脚本暂停，跳过下一个执行阶段，进入下一个执行阶段，跳出当前方法，阻止断点，异常暂停。
* 按钮下面提供Watch(监听)、Call Stack(调用栈)、Scope(作用域)、Breakpoints(断点记录)、DOM Breakpoints、XHR Breakpoints(XHR断点)、EventListener Breakpoints(事件断点)等信息。关于按钮之下的信息在这里不扩开讲，自己可以尝试一下，对于查错很有帮助。这里主要讲下在node环境中，怎么像浏览器环境调试javascript一样调试nodejs代码。
<!-- more -->
* node.js的debugger功能相对浏览器环境来说简单了点，但是能满足基本需要。一步步来：
	* 在对应的js文件中写入debugger，这里以demo.js为例
	```
		/*demo.js*/
		const fs = require('fs')
		const path = require('path')
		var CONF = {
			'searchType':''
		}
		var searchCode = process.argv.splice(2)
		debugger
	```
	* 终端中敲入node debug demo.js, 进入debug模式，有几个命令辅助
		* cont, 简写c, 断点区域
		* next, 简写n, 执行下一块代码，跳过执行过程
		* step, 简写s, 执行并进入下一块代码
		* out, 简写o, 跳出当前方法
		* pause, 暂停断点
		* quit, 离开，离开后不会执行当前行数之后的代码
	* 除了可以在脚本中直接声明debugger, 还可以在debug模式下，敲入命令动态加入断点
		* setBreakpoint(), sb(), 在当前行执行断点
		* setBreakpoint(line), sb(line), 在指定行断点
		* setBreakpoint('fn()'), sb(...)
		* setBreakpoint('other.js', line), sb(...), 在指定js文件指定行断点
		* clearBreakpoint('other.js',1), cb(...)，去除指定文件指定行断点
	* 如果想看一些断点信息，也有命令支持
		* backtrace, bt, 打印当前断点片段信息，如文件名，行数，所处第几字符等
		* list(line), 列出当前断点行数前后line行的代码片段
		* watch(expr), 增加表达式到监听队列, 如watch('CONF')
		* unwatch(expr), 从监听队列移除对应表达式
		* watchers, 罗列监听队列中的所有表达式和值，列出对应表达式和值，此例中0：CONF = {"searchType":""}
		* repl, 打开debugger repl查看代码状态，可以在这一步尝试跟踪变量状态
		* exec expr, 执行表达式
	* 可控制运行状态，通过一下命令
		* run
		* restart
		* kill, 终止, 可以通过run或restart重启
	* 其他
		* scripts, 列出所有加载的scripts
		* version, 列出v8引擎的版本