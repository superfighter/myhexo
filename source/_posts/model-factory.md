---
title: model-factory
date: 2016-04-11 21:28:52
tags: model javascript
---

# 设计模式之工厂模式 #


### 角色定义 ###


从武侠游戏角色角度设计，可以是武林至尊、绝世高手、高手、小虾米等，我们可以把它实例化成任和一个角色，并且任意转换。
<!-- more -->
### 生产角色 ###

我们可以通过以下代码实现角色定义：
```javascript
   var Wulin = function(){};
   Wulin.prototype = {
       var person;
       initRole:function (model){
           switch(model){
                case 'king':
                    person = new King();
                    break;
                case 'higher':
                    person = new Gao();
                    break;
                case 'small':
                    person = new Small();
                    break;
                default:
                    person = new Small();
                    break;
           }
       }
       person.initSkills();
       return person;
   }
```
### 工厂模式 ###

我们可以使用工厂模式进行优化，把`createRole`抽离出来，采用单体来实现这个工厂对象
```javascript
   var RoleFactory = {
       createRole:function (model){
           var person;
           switch(model){
                case 'king':
                    person = new King();
                    break;
                case 'higher':
                    person = new Gao();
                    break;
                case 'small':
                    person = new Small();
                    break;
                default:
                    person = new Small();
                    break;
           }
       }
       return person;
   }
```
然后，使用工厂对象：
```javascript
    var Wulin = function(){};
    Wulin.prototype = {
        initRole:function(model){
            var person = RoleFactory.createRole(model);
            person.initSkills();
            return person;
        }
    }
```
### 小结 ###
以上是简单的工厂模式使用，相比没有使用工厂模式的代码片段的好处是角色相关信息都集中在一个地方管理，在新增一个角色时不影响`Wulin`这个大类的代码，后者相比前者更容易维护`Wulin`类和角色类型。