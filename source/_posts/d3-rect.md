---
title: D3-Rect
date: 2017-05-30 21:14:23
tags:
	- d3
---
* 一段时间没有记录博客了，四月份和五月份都很忙，四月份主要忙weex，五月份主要忙wexx+d3.v3.js，这里先稳下如何用d3实现负柱形图，供以后参考。
### 基础元素 ###
* 创建底盘svg
````
	var width = window.innerWidth;
	var height = width/2;
	var svg = d3.selectAll('body')
		.append('svg')
		.attr({
			width: width,
			height: height
		})
		.style({
			background: "black"
		})
````
* 声明元数据，比例尺，坐标轴
````
	var dataSet = [10,20,30,40,60]
	var xScale = d3.scale.ordinal()
		.domain(dataSet)
		.rangeRoundBands([0,width-margin.right-margin.left])
	var yScale = d3.scale.ordinal()
		.domain(dataSet)
		.rangeRoundBands([height-margin.bottom-margin.top,0])
	var xAxis = d3.svg.axis()
		.scale(xScale)
		.orient('bottom')
	var yAxis = d3.svg.axis()
		.scale(yScale)
		.orient('left')
````
* 生成坐标轴
````
	svg.append('g')
		.call(xAxis)
	svg.append('g')
		.call(yAxis)
````
* 因为没有设置合理的位置，Y轴在可见区域之外了，X轴在最顶上了，这都是不是我们期望的，下面做一些改动。
````
	// 这里额外定义了偏值
	var margin = {
		'top':20,
		'right': 50,
		'bottom': 50,
		'left': 50
	};
	var width = window.innerWidth;
	var height = width/2;
	var svg = d3.selectAll('body')
    .append('svg')
    .attr({
        width: width,
        height: height
    })
    .style({
        background: "#FFF"
    })
	var dataSet = [1,2,3,4,5]
	var xScale = d3.scale.ordinal()
		.domain(dataSet)
		.rangeRoundBands([0,width-margin.right-margin.left])
	var yScale = d3.scale.ordinal()
		.domain(dataSet)
		.rangeRoundBands([height-margin.bottom-margin.top,0])
	var xAxis = d3.svg.axis()
		.scale(xScale)
		.orient('bottom')
		.tickSize(-100,0)
	var yAxis = d3.svg.axis()
		.scale(yScale)
		.orient('left')
		.tickSize(-100,-(width-margin.right-margin.left))

	svg.append('g')
		.attr('transform', 'translate('+(margin.left)+','+(height-margin.bottom)+')')
		.call(xAxis)
	svg.append('g')
		.attr('transform', 'translate('+margin.left+','+margin.top+')')
		.call(yAxis)
````
* 以上，我们最简单的打底内容就搞定了，下面尝试添加柱形元素。
### 正向柱形 ###
````
	svg.selectAll('rect').data(dataSet).enter()
		.append('rect')
		.attr({
			'class': 'rect',
			'x': function(d, i){
				return xScale(d)+margin.left
			},
			'y': function(d, i){
				return yScale(d) + margin.top
			},
			'width': function(d, i){
				return  xScale.rangeBand()
			},
			'height': function(d, i){
				return height - margin.top - margin.bottom - yScale(d)
			},
			'fill': function(d){
				return 'red'
			}
		})
````
* 这样我们就能渲染正常的柱形图
### 负向柱形 ###
* 负向的要点主要有两点
	* 负刻度以0为基准线
	* 负刻度的高度也要以0对应的值为基准值，最后求绝对值
* 代码稍做改动就能达到如期效果
````
	var dataSet = [1,2,-3,4,5]
	var xScale = d3.scale.ordinal()
		.domain(dataSet)
		.rangeRoundBands([0,width-margin.right-margin.left])
	var yScale = d3.scale.linear()
		.domain([d3.min(dataSet,d3.max(dataSet)])
		.range([height-margin.bottom-margin.top,0])
	svg.selectAll('rect').data(dataSet).enter()
		.append('rect')
		.attr({
			'class': 'rect',
			'x': function(d, i){
				return xScale(d)+margin.left
			},
			'y': function(d, i){
				// 要点一，注意这里
				var basePoint = d > -1 ? yScale(d) : yScale(0)
				return basePoint + margin.top
			},
			'width': function(d, i){
				return  xScale.rangeBand()
			},
			'height': function(d, i){
				// 要点二，注意这里
				var baseHeight = yScale(0) - yScale(d)
				return d > -1 ? baseHeight : Math.abs(baseHeight)
			},
			'fill': function(d){
				return 'red'
			}
		})
````
* 暂时写到这里，后续补充：分组柱形图