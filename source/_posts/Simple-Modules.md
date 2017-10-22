---
title: 20176-Simple-Modules
date: 2017-10-22 16:26:51
tags: summary
---
## 前言
---
这篇文字简单的实现了`define`方法，了解`闭包`的使用场景.
<!-- more -->
## Demo
---
````javascript
var MyModuler = (function() {
    var modules = {};
    function define(name, deps, fact) {
        for (var i=0; i<deps.length; i++) {
            deps[i] = modules[deps[i]];
        }
        modules[name] = fact.apply(fact, deps);
    }
    function get(name) {
        return modules[name];
    }
    return {define: define, get: get};
}());
// 定义
MyModuler.define('first', [], function () {
    return {
        'name': 'first name'
    };
})

MyModuler.define('second', ['first'], function (First) {
    return {
        'name': First.name + ' brother'
    };
});
// 调用
console.log(MyModuler.get('second').name);
````
## 小结
很像前端进化史中近代模块化开发的`require`吧，核心是通过`闭包`缓存了`modules`对象。