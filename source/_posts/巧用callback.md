---
title: 巧用callback实现链式调用
date: 2016-04-04 16:17:29
tags: javascript
---
### normall ###

```
window.Normal = window.Normal || function(){
	var name = 'hello';
	//private
	this.setName = function(n){
		name = n;
		return this;
	}
	//private
	this.getName = function(){ return name;}
};
var NM = new Normal;
console.log(NM.getName()); // displays 'hello'
console.log(NM.setName('world').getName());// displays 'world'
```
<!-- more -->
### use callback ###

```
window.Normal = window.Normal || function(){
        var name = 'hello';
        //private
        this.setName = function(n){
                name = n;
                return this;
        }
        //private
        this.getName = function(cb){ 
		cb.call(this,name);
		return this;
	}
};      
function log(s){ console.log(s);}
var NM = new Normal;
NM.getName(log).setName('world').getName(log); // displays 'hello' then 'world'
```

