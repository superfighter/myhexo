---
title: a pixel is not a pixel is not a pixel
date: 2016-07-01 20:18:16
tags:
	- css
---
原文[A pixel is not a pixel is not a pixel](http://www.quirksmode.org/blog/archives/2010/04/a_pixel_is_not.html)
* 现在我正在深入研究移动端中跨浏览器手机的宽和高，得出的结论是在99%的情况下，这些因素不会影响到正常的web开发
* 剩余的1%变得非常诡异，我期望苹果公司通过假如一个像素层来解决这个1%引起的问题
* 开始之前，我先提示下：因为这些因素对那些我忽视的正规屏幕尺寸并不重要，并且我没有认真的去研究过展示、像素密度等其他复杂概念。所以，接下的内容我可能用错了很多专业术语，多多包涵。

#### Web开发者需要什么 ####
* 我并不知道web开发者真正感兴趣的是什么，但是我知道他们需要CSS像素单位。就是在CSS表达式中声明的：width: 300px 或者是 font-size: 14px;
* CSS像素和设备像素密度没有任何关系，也跟即将产生的中间层没有任何关系。他们本质上是专门为我们web开发者创造的抽象构造。
* 我们可以用zoom操作来做简单说明。当用户放大的时候，一个`width:300px`的元素占据了更大的屏幕，这使得被放大的元素在设备（物理）像素上就变得越来越宽。zoom的效果完全是通过尽可能的扩展CSS像素产生的，然而此时该元素的CSS像素值仍是300px。
* 当zoom效果达到100%时，一个CSS像素等于一个设备像素（尽管中间层会替代设备像素层）我得说明下`zoom 100%`对于web开发者来说没有任何意义，Zoom层级对我们不重要，我们要关心的是适配当前屏幕需要多少CSS像素。
* 我们想想当用户zoom操作时到底发生了什么？当用户缩小时，CSS像素变小，此时一个设备像素重叠了更多的CSS像素。当用户放大时，CSS像素变大，此时一个设备像素重叠更少的CSS像素。
* 这样我们的`width:300px`元素的实际CSS像素总是300，另外由zoom因素决定多少设备像素呈现一个CSS像素。
* 这种机制不会改变，如果改变了，所有iPhone网站会严重失控，苹果公司肯定不会让这一切发生的。
* 因此，完全适配的网站扔展示980px，此时我们并不关心多少设备像素来呈现980px。

#### 解决方案 ####
* &lt;meta name=\"viewport\" width=\"device-width\"&gt;
* media query `device-width`
* 以上两种方案都作用于设备像素，而不是CSS像素，因为他们的上下文是网页，不是CSS内部运行环境。

#### media query####
* `device-width`代表设备的宽度。
* `width`代表页面的宽度。
````
/*正常范围宽300*/
div.sidebar { width: 300px;}
/*小于320px设定宽100*/
@media all and (max-device-width: 320px){
	div.sidebar{ width: 100px}
}
````
#### meta tag ####
* 一般来说meta标签更有用。这起先是由苹果发布的属性，后来被许多移动浏览器实现，让layout viewport适配设备屏幕更容易。
* 什么是Layout Viewport? 浏览器用它来计算元素的展示比例（CSS像素）,比如`div.sidebar{width: 20%}`,Layout Viewport总是比设备像素大一些。
* 如果你添加了meta标签，layout viewport的宽度会限制在设备像素内：iPhone 是320
* 即使你的页面小到足以适配屏幕，那也会有影响。应该加上<meta >标签，让文字适配当前屏幕。

#### other ####
* dips: 谷歌提出一个概念，device-independent pixels，CSS像素作用于这一层 