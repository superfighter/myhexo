---
title: closure
date: 2016-07-26 16:39:14
tags:
	- javascript
	- closure
---
内容主要来源于网络文章[4byte](http://www.4byte.cn/learning/119650/ni-bu-yi-ding-neng-zuo-dui-de-javascript-bi-bao-mian-shi-ti.html)
* 文章主旨是为了进一步理解闭包如果运作，最好是先自己动脑想一下答案再看解题思路
* 直接抛题
````
function func(closure, more){
    log(more);
    return {
        func: function(innerClosure){
            return func(innerClosure, closure);
        }
    };
}
function log(s){
    var _d = document.createElement('div');
    _d.innerHTML = s;
    document.body.appendChild(_d);
}
var temp = func(0);
temp.func(1);
temp.func(2);
temp.func(3);
var temp2 = func(0).func(1).func(2).func(3);
var temp3 = func(0).func(1);
temp3.func(2);
temp3.func(3);
//Question: 以上函数执行后出现在body上的结果是什么？
````
<!-- more -->
### 基础知识 ###
* 先了解声明函数的方式
	* `function fn1(){}`
	* `var fn1=function (){}`
	* `var fn1=function xxcanghai(){};`
	* `new Function("alert(987654321)")`
	* `(function(){alert(1);})();(function fn1(){alert(1);})();`
* 要点：理解作用域
````
var o={
  fn:function (){
    console.log(fn);
  }
};
o.fn();//ERROR报错
/*提升*/
var fn=function (){
  console.log(fn);
};
fn();//function (){console.log(fn);};正确
````
### 解析思路 ###
* 解析temp

	* 可以得知，第一个func(0)是在调用第一层func函数。第二个func(1)是在调用前一个func的返回值的func函数

	* 后面几个func(1),func(2),func(3),函数都是在调用第二层func函数。

	* 在第一次调用func(0)时，more为undefined；

	* 第二次调用func(1)时innerClosure为1，此时func闭包了外层函数的closure，也就是第一次调用的closure=0，即innerClosure=1，closure=0，并在内部调用第一层func函数func(1,0);所以more为0；

	* 第三次调用func(2)时innerClosure为2，但依然是调用temp.func，所以还是闭包了第一次调用时的时innerClosure为1，此时func闭包了外层函数的closure，也就是第一次调用的closure，所以内部调用第一层的func(2,0);所以more为0

	* 第四次同理；

	* 最终答案为undefined,0,0,0

* 解析temp2

	* 先从func(0)开始看，肯定是调用的第一层func函数；而他的返回值是一个对象，所以第二个func(1)调用的是第二层func函数，后面几个也是调用的第二层func函数。

	* 在第一次调用func(0)时，more为undefined；

	* 第二次调用func(1)时innerClosure为1，此时func闭包了外层函数的closure，也就是第一次调用的closure=0，即innerClosure=1，closure=0，并在内部调用第一层func函数func(1,0);所以more为0；

	* 第三次调用 .func(2)时innerClosure为2，此时当前的func函数不是第一次执行的返回对象，而是第二次执行的返回对象。而在第二次执行第一层func函数时(1,0)所以closure=1,more=0,返回时闭包了第二次的closure，遂在第三次调用第三层func函数时innerClosure=2,closure=1，即调用第一层func函数func(2,1)，所以more为1；

	* 第四次调用 .fun(3)时innerClosure为3，闭包了第三次调用的closure，同理，最终调用第一层func函数为func(3,2)；所以more为2；

	* 最终答案为undefined,0,1,2

* 解析temp3
	* func(0)为执行第一层func函数，.func(1)执行的是func(0)返回的第二层func函数，这里语句结束，temp3存放的是func(1)的返回值，而不是func(0)的返回值，所以temp3中闭包的也是func(1)第二次执行的closure的值。temp3.func(2)执行的是func(1)返回的第二层func函数，temp3.func(3)执行的也是func(1)返回的第二层func函数。
	* 在第一次调用第一层func(0)时，more为undefined；
	* 第二次调用 .func(1)时innerClosure为1，此时func闭包了外层函数的closure，也就是第一次调用的closure=0，即innerClosure=1，closure=0，并在内部调用第一层func函数func(1,0);所以more为0；
	* 第三次调用 .func(2)时innerClosure为2，此时func闭包的是第二次调用的closure=1，即innerClosure=2，closure=1，并在内部调用第一层fun函数func(2,1);所以more为1；
	* 第四次.func(3)时同理，但依然是调用的第二次的返回值，最终调用第一层func函数func(3,1)，所以more还为1
	* 最终答案：undefined,0,1,1
### 不混淆版 ###

*  转成这样会好理解些
````
function someFunc(closure, more){
    log(more);
    return {
        innerFunc: function(innerClosure){
            return someFunc(innerClosure, closure);
        }
    };
}
function log(s){
    var _d = document.createElement('div');
    _d.innerHTML = s;
    document.body.appendChild(_d);
}
var temp = someFunc(0);
temp.innerFunc(1);
temp.innerFunc(2);
temp.innerFunc(3);
var temp2 = someFunc(0).innerFunc(1).innerFunc(2).innerFunc(3);
var temp3 = someFunc(0).innerFunc(1);
temp3.innerFunc(2);
temp3.innerFunc(3);
````
