---
title: css-box
date: 2016-06-01 14:55:23
tags:
	- css
---
### 容器属性 ###
* box-pack：定义子元素主轴对齐方式：justify-content
````
	box-pack: start | end | center | justify;
    /*主轴对齐：左对齐（默认） | 右对齐 | 居中对齐 | 左右对齐*/
````
* box-align：定义元素交叉轴对齐方式：align-items
````
	box-align: start | end | center | baseline | stretch;
	/*交叉轴对齐：顶部对齐（默认） | 底部对齐 | 居中对齐 | 文本基线对齐 | 上下对齐并铺满*/
````
* box-direction：定义元素显示方向：flex-flow
````
	box-direction: normal | reverse | inherit;
	/*显示方向：默认方向 | 反方向 | 继承子元素的 box-direction*/
````
* box-orient：定义子元素水平或垂直排列：flex-flow
````
	box-orient: horizontal | vertical | inline-axis | block-axis | inherit;
	/*排列方向：水平 | 垂直 | 行内方式排列（默认） | 块方式排列 | 继承父级的box-orient*/
````
* box-lines：定义子元素超出容器是否换行：flex-wrap
````
	box-lines: single | multiple;
	/*允许换行：不允许（默认） | 允许*/
````
* box-flex：定义是否允许子元素伸缩：flex
````
	box-flex: <value>;
	/*伸缩：<一个浮点数，默认为0.0，即表示不可伸缩，大于0的值可伸缩，柔性相对>*/
````
* box-ordinal-group：定义子元素的显示顺序：flex-order
````
	box-ordinal-group: <integer>;
	/*显示次序：<一个整数，默认为1，数值越小越排前>*/
````
* 元素对其方式
````
	align-self: auto | flex-start | flex-end | center | baseline | stretch;
	/*单独对齐方式：自动（默认） | 顶部对齐 | 底部对齐 | 居中对齐 | 上下对齐并铺满 | 文本基线对齐*/
````
### 兼容写法 ###

* 如果子元素是行内元素，在很多情况下都要使用 display:block 或 display:inline-block 把行内子元素变成块元素（例如使用 box-flex 属性），这也是旧版语法和新版语法的区别之一。
* 由于旧版语法并没有列入W3C标准，所以这里不用写 display:box ，下面的语法也是一样的。
````
	.box{
		display: -webkit-box; /* 老版本语法: Safari, iOS, Android browser, older WebKit browsers. */
		display: -moz-box; /* 老版本语法: Firefox (buggy) */
		display: -ms-flexbox; /* 混合版本语法: IE 10 */
		display: -webkit-flex; /* 新版本语法: Chrome 21+ */
		display: flex; /* 新版本语法: Opera 12.1, Firefox 22+ */
	}
````
* 子元素轴对齐方式，旧版语法有4个参数，而新版语法有5个参数，兼容写法新版语法的 space-around 是不可用的
````
	.box{
		-webkit-box-pack: center;
		-moz-justify-content: center;
		-webkit-justify-content: center;
		justify-content: center;
	}
````
* 子元素交叉轴对齐方式
````
	.box{
		-webkit-box-align: center;
		-moz-align-items: center;
		-webkit-align-items: center;
		align-items: center;
	}
````
* 子元素的显示方向，可通过 box-direction + box-orient + flex-direction 实现
````
    /*左到右*/
	.box{
		-webkit-box-direction: normal;
		-webkit-box-orient: horizontal;
		-moz-flex-direction: row;
		-webkit-flex-direction: row;
		flex-direction: row;
	}
	/*右到左*/
	.box{
		-webkit-box-pack: end;
		-webkit-box-direction: reverse;
		-webkit-box-orient: horizontal;
		-moz-flex-direction: row-reverse;
		-webkit-flex-direction: row-reverse;
		flex-direction: row-reverse;
	}
	/*上到下*/
	.box{
		-webkit-box-direction: normal;
		-webkit-box-orient: vertical;
		-moz-flex-direction: column;
		-webkit-flex-direction: column;
		flex-direction: column;
	}
	/*下到上*/
	.box{
		-webkit-box-pack: end;
		-webkit-box-direction: reverse;
		-webkit-box-orient: vertical;
		-moz-flex-direction: column-reverse;
		-webkit-flex-direction: column-reverse;
		flex-direction: column-reverse;
	}
````
* 是否允许子元素伸缩
````
	/*扩展*/
	.item{
		-webkit-box-flex: 1.0;
		-moz-flex-grow: 1;
		-webkit-flex-grow: 1;
		flex-grow: 1;
	}
	/*收缩*/
	.item{
		-webkit-box-flex: 1.0;
		-moz-flex-shrink: 1;
		-webkit-flex-shrink: 1;
		flex-shrink: 1;
	}
````