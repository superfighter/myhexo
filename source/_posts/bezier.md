---
title: bezier
date: 2017-03-02 10:23:10
tags:
    - bezier
---
# 简介
是用一系列点来控制曲线状态
* 数据点，控制曲线的起始位置和结束位置
* 控制点，确定曲线的弯曲程度
## 一阶曲线
没有控制点，仅有两个数据点，最终效果一个线段
## 二阶曲线
两个数据点A和C，一个控制点B来描述曲线状态
* 链接AB，BC
* 在AB上取点D，BC上取点E，使AD/AB = BE/BC
* 链接DE，取点F，使AD/AB = BE/BC = DF/DE
## 三阶曲线
两个数据点A和D，两个控制点来描述曲线状态

