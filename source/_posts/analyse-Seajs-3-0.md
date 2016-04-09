---
title: analyse Seajs-3.0
date: 2016-04-09 17:56:46
tags: seajs javascript
---

	1、检测seajs对象
	2、初始化seajs对象
		* data
	3、lang 辅助方法
		* isObject
		* isString
		* isArray
		* isFunction
		* isUndefined
	4、_cid变量自增
	5、data.events对象
	6、定义事件绑定、解绑方法
	7、emit方法定义触发事件
	8、path 辅助方法
		* 目录正则
		* 同级正则
		* 上级正则
		* 多斜杠正则
		* 序列化绝对地址
		* 正常化模块js地址
		* 路径正则
		* config中配置的vars正则/*{}*/
		* 解析配置中alias属性值
		* 解析配置中paths属性值
		* 解析配置中vars的属性值
		* 解析配置中map的属性值
		* 定义追加base路径方法
		* 定义id2uri方法
<!-- more -->
	9、seajs.resolve = id2uri
	10、定义判定环境变量
	11、忽略本地路径判断正则
	12、定义当前路径变量
	13、定义当前模块所处地址
	14、定义request方法，插入script标签加载对应模块路径
		* 定义addOnload方法
			* 定义onload方法
	15、seajs.request = request
	16、解析依赖deps方法
		* 处理空格
		* 处理单双引号
		* 处理peek
			* 处理正则
		* 处理单词
		* 处理数字
		* 处理左括号
		* 处理右括号
		* 处理左大括号
		* 处理右大括号
		* 返回res
	17、seajs.cache 属性
	18、定义匿名Meta变量、正在抓取列表、已抓取列表、回调列表
	19、定义状态码
	20、定义Module构造函数
		* 处理依赖，返回地址数组
		* 验证是否入栈？
		* 加载模块依赖方法
		* 加载后执行方法
		* 出错方法
		* 解析模块代码
		* 抓取模块方法
		* 定义模块的id2uri方法
		* define方法
		* save方法保存define定义的模块<存到cachedMods>
		* get获取module
		* use使用等同于加载匿名模块
	21、seajs.use
	22、定义define.cmd和define
	23、seajs.require
	24、处理config