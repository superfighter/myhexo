---
title: nbsp
date: 2016-09-07 14:26:12
tags:
	- html
---
* 编写静态HTML页面的时候时不时会遇到空格对其的情况，比如Form表单的title对其，因为中文字体不同宽度不同，完美处理要搞事。
* 在HTML中有5种空格实体。他们拥有不同的宽度，经常使用的是"&amp;nbsp;"，这个大家都熟悉，是由空格键输入的结果。除了这个，其他还有（&amp;ensp;&amp;emsp;&amp;thinsp;&amp;zwnj;&amp;zwj;）
### &amp;nbsp; ###
* 全称No-Break Space，占据宽度受字体影响明显。
### &amp;ensp; ###
* 半角空格，全称En Space，占据宽度1/2中文宽度，基本上不受字体影响。
### &amp;emsp; ###
* 全角空格，全称Em Space，占据宽度1/1中文宽度，基本上不受字体影响。
### &amp;thinsp; ###
* 窄空格，全称Thin Space，占据1/6 em宽度。
### &amp;zwnj; &amp;zwj; ###
* 零宽不连字和零宽连字，多用于打印排版
### 其他 ###
* 空格(&amp;#x0020;)、制表位(&amp;#x0009;)、换行(&amp;#x000A;)和回车(&amp;#x000D;)(&amp;#12288;)等；
