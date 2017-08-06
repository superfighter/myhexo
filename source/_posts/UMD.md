---
title: UMD
date: 2016-12-22 10:08:35
tags: 
	- module
	- node
	- jQuery
	- AMD
	- commonJS
---
[参考自github/umdjs](https://github.com/umdjs/umd)
* UMD (Universal Module Define) ，即通用模块定义。目前已有的模块定义规范有commonJS,Node,AMD,CMD,全局挂载等方式，如何同时适配两种或多种，做到一劳永逸是有套路的，具体尝试如下：
* A:基于AMD和全局挂载的
````
	(function(root, factory){
		if(typeof define === 'functon' && define.amd){
			define(['b'], factory);
		}
		else {
			root.amdWeb = factory(root.b);
		}
	}(this, function(b){
		return {}
	}))
````
* 在A的基础上挂载全局
````
	/*当你使用AMD模式的时候，有些脚本还是
	要去加载全局变量，这时需要在AMD模式尾巴上挂载全局变量*/
	(function(root, factory){
		if(typeof define === 'function' && define.amd){
			define(['b'], function(b){
				//do something
				return (root.amdWebGlobal = factory(b))
			})
		}
		else {
			root.amdWebGlobal = factory(root, b)
		}
	}(this, function(b){
		return {}
	}))
````
<!-- more -->
* B基于CommonJS和AMD
````
	/*全局挂载失效*/
	if(typeof exports === 'object' && typeof exports.nodeName !=='string' && typeof define !== 'function'){
		var define = function (factory){
			factory(require, exports, module);
		}
	}

	define(function(require, exports, module){
		var b = require('b');
		exports.action = function(){};
	})
````
* 在B的基础上挂载全局
````
	(function (root, factory){
		if(typeof define === 'function' && define.amd){
			define(['exports', 'b'], factory);
		}
		else if(typeof exports === 'object' && typeof exports.nodeName !== 'string'){
			factory(exports, require('b'));
		}
		else {
			factory((root.commonJsStrict = {}), root.b);
		}
	}(this, function(exports, b){
		exports.action = function(){};
	}))
````
* 同A一样，在AMD模式时全局挂载就要这样做：
````
	(function (root, factory){
		if(typeof define === 'function' && define.amd){
			define(['exports', 'b'], function(exports,b){
				factory((root.commonJsStrictGlobal = exports),b)
			})
		}
		else if(typeof exports === 'object' && typeof exports.nodeName !== 'string'){
			factory(exports, require('b'))
		}
		else {
			factory((root.commonJsStrictGlobal={}), b);
		}
	}(this, function(exports, b){
		exports.action = function (){};
	}))
````
* C类似A，基于Node,AMD,全局挂载。不同的是C导出exports变量，
````
	(function (root, factory){
		if(typeof define === 'function' && define.amd){
			define(['b'], factory)
		}
		else if(typeof module === 'object' && module.exports){
			module.exports = factory(require('b'))
		}
		else {
			root.returnExports = factory(root.b)
		}
	}(this, function(b){
		return {}
	}))
	/**很多时候需要定义无依赖模块，就像下面这样**/
	(function (root, factory){
		if(typeof define === 'function' && define.amd){
			define([], factory)
		}
		else if(typeof module === 'object' && module.exports){
			module.exports = factory()
		}
		else {
			root.returnExports = factory()
		}
	}(this, function(){
		return {}
	}))
````
* 同上面的A、B类似，在AMD模式下有时需要全局挂载的时候，要对C稍作改动
````
	(function(root, factory){
		if(typeof define === 'function' && define.amd){
			/**重点这里**/
			define(['b'], function(b){
				return (root.returnExportsGlobal = factory(b));
			})
		}
		else if(typeof module === 'object' && module.exports){
			module.exports = factory(require('b'))
		}
		else{
			//root 是 browser globals, 如window
			root.returnExportsGlobal = factory(root.b)
		}
	}(this, function(b){

	}))
````
* 最后看下jQuery Plugin怎么适配进来，重点是弄清楚jQuery对象的引用
````
	(function(factory){
		if (typeof define === 'function' && define.amd){
			define(['jquery'], factory);
		}
		else if(typeof module === 'object' && module.exports){
			module.exports = function(root, jQuery){
				if(jQuery === undefined){
					if(typeof window !== 'undefined'){
						jQuery = require('jquery');
					}
					else {
						jQUery = require('jquery')(root)
					}
				}
				factory(jQuery);
				return jQuery;
			}
		}
		else {
			factory(jQuery);
		}
	}(function($){
		$.fn.jqueryPlugin = function(){return true;};
	}))
````