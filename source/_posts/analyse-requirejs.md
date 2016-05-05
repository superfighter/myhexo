---
title: analyse-requirejs
date: 2016-04-26 08:51:03
tags: 
	- requirejs
	- javascript
---
### 定义常量 ###
req
s
head
baseElement
dataMain
src
interactiveScript
currentlyAddingScript
mainScript
subPath
version
commentRegExp
jsSuffixRegExp
op
ostring
hasOwn
isBrowser
isWebWorker
readyRegExp
defContextName
isOpera
contexts
cfg
globalDefQueue
useInteractive
handlers
<!-- more -->
### 定义方法 ###
commentReplace
isFunction
isArray
each
eachReverse
hasProp
getOwn
eachProp
mixin
bind
scripts
defaultOnError
getGlobal
makeError
newContext
### 定义类 ###
newContext
	* inCheckLoaded
	* Module
	* context
	* handlers
	* checkLoadedTimeoutId
	* config
		* waitSeconds
		* baseUrl
		* paths
		* bundles
		* pkgs
		* shim
		* config
	* registry
	* enabledRegistry
	* undefEvents
	* defQueue
	* defined
	* bundlesMap
	* requireCounter
	* unnormalizedCounter
	* fun trimDots
	* fun normalize ****
	* fun removeScript
	* fun hasPathFallback
	* fun splitPrefix
	* fun getModule
	* fun cleanRegistry
	* fun breakCycle
	* fun checkLoaded
Module
	* this.events
	* this.map
	* this.depExports
	* this.depMaps
	* this.depMatched
	* this.pluginMaps
	* this.depCount
	* init
		* this.errback
		* this.inited
		* this.ignore
		* this.factory
	* defineDep
	* fetch
	* on
	* emit
	* load
	* check
	* callPlugin
	* enable
	* intakeDefines
	* takeGlobalQueue
context
 * config
 * contextName
 * registry
 * defined
 * urlFetched
 * defQueue
 * defQueueMap
 * Module
 * makeModuleMap
 * nextTick
 * onError
 * makeShimExports
 * makeRequire
 * enable
 * completeLoad
 * nameToUrl
 * load
 * execCb
 * onScriptLoad
 * onScriptError
 * configure
 	* baseUrl
 	* urlArgs
 	* shim
 	* bundles
 	* packages
 	* deps
 	* callback
 	* skipDataMain
### 从上至下逻辑 ###
检测是否存在AMD loder
检测是否存在requirejs接口
检测是否存在require配置对象
	* req.config 支持require.config协作其他AMD加载器
	* req.nextTick 定时执行
	* 如果不存在require对象，赋值req
	* req.version 版本
	* req.jsExtRegExp
	* req.isBrowser
	* req.s
	* req({}) 创建默认上下文
	* 给全局req对象导入一些人性化方法
	* 如果是浏览器环境，赋值s.head为head容器
	* req.onError
	* req.createNode
	* req.load
	* 设定base路径
	* req.exec
定义define
定义define.amd

加载完所需module后加载config.js执行require.config()?
normalize-->name2url-->enable-->check-->fetch-->module.load-->context.load-->req.load最后加载map.url