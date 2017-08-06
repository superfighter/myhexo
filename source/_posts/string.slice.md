---
title: string.slice
date: 2016-04-01 09:02:21
tags: javascript
categories: string
---

* 简单介绍补零方案

```
	function pad(s){
		return ('0' + s).slice(-2);
	}
```
<!-- more -->
* 使用

```
	//如果满足大于一位字符串，则取最后两位 
	//不满足就在前面补0
	//适用于时间、日期格式
	pad(1);// 01
	pad(12);// 12
```