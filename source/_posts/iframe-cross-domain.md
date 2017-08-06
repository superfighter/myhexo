---
title: iframe cross-domain
date: 2017-02-07 15:32:04
tags:
	- iframe
	- cross-domain
---

* 这篇文章其实不是为解决跨域请求而写的，主要是为了处理相同顶级域名下，不同子域间如何沟通，以及iframe自适应自身内容高度的问题。
* 主要思路是借用`document.domain`来统一域名，达到浏览器的`同源策略`要求。
* 本例需求还涉及到对clientHeight、offsetHeight、scrollHeight的理解。
* 这里不展开介绍contentWindow、contentDocument以及ie兼容(attachEvent)的情况，可以自行查阅资料。
<!-- more -->
* 核心代码如下：
````
	function setIframeHeight(iframe) {
        if (iframe) {
        	var iframeWin = iframe.contentWindow || iframe.contentDocument.parentWindow;
            if (iframeWin.document.body) {
            	iframe.height = iframeWin.document.documentElement.scrollHeight || iframeWin.document.body.scrollHeight;
            }
        }
    }

    window.onload = function(){
		setIframeHeight(document.querySelector('#iframe'))
	}
````
* 如果希望跨主域，网络上有成熟的方案，核心思想是通过中间页（c）链接请求页面（a）和响应页面（b），其中a和c是同一主域，不受同源策略影响，a和b通过url方式传参，b和c也通过url方式传参，最终达到a请求b后从c返回数据给a。
* 在跨主域问题上，如果a想操作iframe（b）的DOM，那是不能做到的，因为受同源策略限制。