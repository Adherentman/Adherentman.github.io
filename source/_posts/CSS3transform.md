---
title: CSS3transform/transition
date: 2018-03-15 21:52
updated: 2018-04-16 12:19:18
comments: true
layout: post
tags: [CSS]
categories: CSS
---
# CSS3 transform

先来了解下单位：

* 度（deg）。一个圆360度

Transform就指变换。

transform:

* 矩阵  (matrix)
  * 用六个指定的值来指定一个均匀的二维（2D）变换矩阵
* 转换（translate）
* 旋转（rotate）
* 缩放（scale）
* 倾斜（skew）

<!--more-->

但是还可以这样玩

> scaleX, scaleY
>
> skewX, skewY
>
> translateX, translateY
>
> matrix3d
>
> `matrix(a, b, c, d, tx, ty)` 是 `matrix3d(a, b, 0, 0, c, d, 0, 0, 0, 0, 1, 0, tx, ty, 0, 1)` 的简写

* **transform-origin**
  * 以一个点去变形
* **transform-style**
  * 确定元素的子元素是否位于3D空间中，还是在该元素所在的平面内被扁平化。


# CSS transition

`transition = <transition-property> | <transition-duration> | <transition-delay> | <transition-timing-function>`

* Transition-property: 指定过渡的属性值。
* Transition-duration: 指定这个过渡的持续时间
* Transition-delay: 延迟过渡时间
* Transition-timing-function: 指定过渡动画运行类型；
  * ease	越来越慢
  * linear      匀速
  * ease-in    先慢后快
  * ease-out 先快后慢 
  * ease-in-out 先慢后快再慢
  * cubic-bezier()



先来看一个transform和transition结合的demo：[字体歪斜变正](https://codepan.net/gist/02e3a0857615ee407878f1b46663f950)

