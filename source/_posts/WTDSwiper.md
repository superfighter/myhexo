---
title: WTDSwiper
date: 2016-12-22 13:59:59
tags:
	- swiper
	- touch
---
* 隐藏内部实现
* 依靠你能控制的对象，好过依靠你不能控制的对象，以免日后受他控制

* 类
- 单一权责原则：只有一条修改理由
- 开放-闭合原则：只许扩展，不许修改
- 低耦合高内聚原则
- 依赖倒置原则：依赖于抽象，而不是具体实现细节
* 并发基础定义
- 限定资源， 并发环境中有着固定尺寸或数量的资源
- 互斥，每一时刻仅有一个县城能访问共享数据或共享资源
- 线程饥饿，一个或一组线程在很长时间内或永久被禁止
- 死锁 两个或多个线程互相等待执行结束
- 活锁 执行次序一致的线程，每个都想要起步，但发现其他线程已经“在路上”
* 并发模型
- 生产者-消费者模型
- 读者-作者模型
- 宴席哲学家
* [[知乎](https://zhuanlan.zhihu.com/p/21927991)]

````
	//两点触控旋转角度和方向求解代码
	//这两个方法属于向量计算，具体原理请阅读本文最后的参考文献

	_getRotateDirection(vector1,vector2) {

		return vector1.x * vector2.y - vector2.x * vector1.y;

	}  

	_getRotateAngle(vector1,vector2) {

		let direction = this._getRotateDirection(vector1,vector2);

		direction = direction > 0 ? -1 : 1;

		let len1 = this._getDistance(vector1.x,vector1.y);

		let len2 = this._getDistance(vector2.x,vector2.y);

		let mr = len1 * len2;

		if(mr === 0) return 0;

		let dot = vector1.x * vector2.x + vector1.y * vector2.y;

		let r = dot / mr;

		if(r > 1) r = 1;

		if(r < -1) r = -1;

		return Math.acos(r) * direction * 180 / Math.PI;

	}
````