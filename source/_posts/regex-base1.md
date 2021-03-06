---
title: 词法分析基础
date: 2016-04-26 08:48:20
tags:
	- regExp
	- javascript
	- DFA
	- NFA
---
主要参考[陈梓瀚](http://www.cppblog.com/vczh/)
#### 问题概述 ####
* 词法分析的重要性
	* 运行命令
	* (1+2)*(3+4)
		* (左括号,"(") (数字,"1") (一级操作符,"+") (数字,"2") (右括号,")") (二级操作符,"*") (左括号,"(") (数字,"3") (一级操作符,"+") (数字,"4") (右括号,")")
		* 操作符、括号、数字
		* 但是，都是等价的对象，没有层次
#### 正则表达式 ####
* 代表字符串集合的规则
	* 规则可以把一个特定的字符或者是空字符串认为是一种类型的记号的全部，如"("是"左括号"，"["不会是"左括号"
	* 规则可以进行串联，"hello"是"h","e","l","l","o"的记号串联
	* 规则可以进行并联, "if","else","while"等字符串，称之为关键字
	* 规则是可选的， "abc","abcde"可以看做是"abd"的前缀字符串，"de"是可选的
	* 规则可以重复
<!-- more -->
##### 范式 #####
* 以上规则组合成更大的规则，引入一种范式来形式化表示，这种范式有以下语法：
1：字符用双引号包围起来，空串使用ε代替。
2：两个规则头尾连接代表规则的串联。
3：两个规则使用 | 隔开代表规则的并联。
4：规则使用[]包围代表该规则是可选的，规则使用{}包围代表该规则是重复的。
5：规则使用()包围代表该规则是一个整体，通常用于改变操作符 | 的优先级。
#### 有穷状态机 ####
* 就是把正则表达式转换为机器可以阅读的形式
##### 分析器含义 #####
* 分析器在每一次进行一项新的工作的时候，都要把状态重置为起始状态
* 分析器每读入一个字符就修改一次状态，修改的方法我们也可以指定
* 分析器在读完所有的字符以后，必然停留在一个确定的状态中
* 如果这个状态跟我们所期望的状态一致的话，我们就说这个分析器接受了这个字符串，否则我们就说这个分析器拒绝了这个字符串
##### 分析器案例 #####
* 检查一个字符串是否由偶数个a和偶数个b组成
	* 小写代表奇数个，大写代表偶数个
	* 可分为ab,aB,Ab,AB四种状态
	* 初始状态是空字符，可认为是00个a和b，即AB状态，也是本例结束状态。结束状态可以是一个，也可是多个。
	* 两个字符串：”abaabbba”和”aababbaba”
		* 前者状态机所经过的状态是：AB[a]aB[b]ab[a]Ab[a]ab[b]aB[b]ab[b]aB[a]AB
		* 后者状态机所经过的状态是：AB[a]aB[a]AB[b]Ab[a]ab[b]aB[b]ab[a]Ab[b]AB[a]aB
##### DFA / NFA / ε - NFA #####
##### 正则表达式到 ε - NFA #####
* 字符集
* 并联
* 串联
* 重复
* 可选
###### 消除非确定性 ######
* 找到所有有效状态
	* 有效状态就是在完成了消除ε边算法之后仍然存在的状态
* 添加所有必要的边
	* 一个状态的ε闭包就是从这个状态出发，仅通过ε边就可以到达的所有状态的集合
* 所有从有效状态出发的非ε边都是不能删除的边
###### NFA 到 DFA ######
* 把{S}放进队列L和集合D中。其中S是NFA的起始状态。队列L放置的是未被处理的已经创建了的DFA状态，集合D放置的是已经存在的DFA状态。根据上文的讨论，DFA的每一个状态都对应着NFA的一些状态
* 从队列L中取出一个状态，计算从这个状态输出的所有边所接受的字符集的并集，然后对该集合中的每一个字符寻找接受这个字符的边，把这些边的目标状态的并集T计算出来。如果T∈D则代表当前字符指向了一个已知的DFA状态。否则则代表当前字符指向了一个未创建的DFA状态，这个时候就把T放进L和D中。在这个步骤里有两层循环：第一层是遍历所有接受的字符的并集，第二层是对每一个可以接受的字符遍历所有输出的边计算目标DFA状态所包含的NFA状态的集合
* 如果L非空则跳到2