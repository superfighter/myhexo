---
title: canvasSave
date: 2017-03-01 10:21:15
tags:
    - canvas
---
# 本文主要阐述作者对Canvas中save和restore两个属性的理解。
## save 
* 根据文档说明是把绘制状态保存到栈中，相关状态如下：
    * strokeStyle, fillStyle, globalAlpha, lineWidth, lineCap, lineJoin, miterLimit
    * lineDashOffset, shadowOffsetX, shadowOffsetY, shadowBlur, shadowColor, globalCompositeOperation
    * font, textAlign, textBaseline, direction, imageSmoothingEnabled.

## restore
* 根据文档说明是恢复到最近的保存状态，如果没有保存状态，此方法不做任何改变

## 实例
<!-- more -->
* 以下展示的是（由外而内）：红->蓝->黑->蓝
````
    window.onload = function(){
        var canvas = document.querySelector('canvas')
        var ctx = canvas.getContext('2d')

        ctx.fillStyle = 'red'
        ctx.fillRect(0,0, 150,150)
        ctx.save()

        ctx.fillStyle = 'skyblue'
        ctx.fillRect(10,10, 130,130)
        ctx.save()

        ctx.fillStyle = 'black'
        ctx.fillRect(20,20, 110,110)

        ctx.restore() // 这里释放了 skyblue
        ctx.fillRect(40,40, 70,70)

    }
````
* 以下展示的是（由外而内）：红->蓝->黑->红
````
    window.onload = function(){
        var canvas = document.querySelector('canvas')
        var ctx = canvas.getContext('2d')

        ctx.fillStyle = 'red'
        ctx.fillRect(0,0, 150,150)
        ctx.save()

        ctx.fillStyle = 'skyblue'
        ctx.fillRect(10,10, 130,130)
        ctx.save()
        ctx.restore() // 注意这里释放了 skyblue

        ctx.fillStyle = 'black'
        ctx.fillRect(20,20, 110,110)

        ctx.restore() // 注意这里释放了 red
        ctx.fillRect(40,40, 70,70)

    }
````
* 以下展示的是（由外而内）：红->蓝->黑
````
    window.onload = function(){
        var canvas = document.querySelector('canvas')
        var ctx = canvas.getContext('2d')

        ctx.fillStyle = 'red'
        ctx.fillRect(0,0, 150,150)
        ctx.save()
        ctx.restore() // 注意这里释放了 red

        ctx.fillStyle = 'skyblue'
        ctx.fillRect(10,10, 130,130)
        ctx.save()
        ctx.restore() // 注意这里释放了 skyblue

        ctx.fillStyle = 'black'
        ctx.fillRect(20,20, 110,110)

        ctx.restore() // 注意这里已经没有了
        ctx.fillRect(40,40, 70,70)

    }
````
## 总结
* 理解保存到栈中很重要，栈是后进先出的数据结构，因此先释放最后进来的状态。