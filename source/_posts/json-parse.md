---
title: JSON两个api用法简介
date: 2016-03-30 09:17:05
tags: javascript json
---

```
var preJSON = "{\"name\":\"wang\",\"age\":\"23\"}";
var simpleJSON = JSON.parse(preJSON);//简单的转换
//第二个参数(官网称为reviver)是一个函数, 在每个键/值在每一级的最终结果后都会被调用. 每个值将会被替换为reviver函数的返回结果
var dueJSON = JSON.parse(preJSON,function(key,value){
if(key !== ""){//这里要判定key值是否为空，不然遍历到最后key为空，value值为Object
value = value + '!';
}
return value;//这里必须返回value
});
var jsonToString1 = JSON.stringify(dueJSON, ['name']);//第二个参数是筛选值，是个数组，这里返回"{"name":"wang"}"
var jsonToString2 = JSON.stringify(dueJSON, null);//第二个参数设置为null则全部转换
var jsonToString3 = JSON.stringify(dueJSON, null, 4);//第三个参数为开头格式化值（空格），这里4就是以4个空格为准格式化转换后的字符串
```
   
