---
title: regExp for ID card
date: 2016-03-31 12:26:10
tags: 
	- regExp
	- javascript
---

* 要处理的字符串
		
		卫*怡 12000019880919801X
		汤*功 522635198308283119
		黄*悦 451025197206119300
		匿名 110105710923582
	要求从中取出所有的身份证
<!--more-->
* 正则：`\d{17}(?:[xX])|\d{15,18}`
* 执行

		变量.match(/\d{17}(?:[xX])|\d{15,18}/igm)
* 得到

		["12000019880919801X", 
		"522635198308283119", 
		"451025197206119300", 
		"110105710923582"]
* 分析

	* 思路
		* 身份证有十五位和十八位
		* 十八位里分全数字和数字加Xx
	* 解析
		* `\d{17}(?:[xX])`
			* ?: 是分组，把项组合到一个单元，但不记忆与该组相匹配的字符，不生成引用
			* 如果换成 `?=` 则不会匹配到xX
		* `|` 
		* `\d{15,18}`
			* 简单匹配15~18个数字
