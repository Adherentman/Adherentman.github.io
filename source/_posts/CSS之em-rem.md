---
title: CSS之em/rem
date: 2018-08-13 18:51
comments: true
layout: post
tags: CSS
categories: CSS
---

# CSS之em/rem
#blog
先来个一句话概括：
- `em`相对于父元素
- `rem`相对于根元素
所以这些都是相对单位。

默认`font-size`为16px所以我们可以知道`1px和1em`之间的关系
> 1em = 16px  
> 1px = 1 ÷ 16 = 0.0625em  

那么我们知道具体的px值后我们就能直接换算，比如：
> 我想要800px转换成`em`  
> 800 * 0.0625em = 50em  

那么如果父元素不为16px，根据上面我们可以得出一个公式：
> 1 ÷ 父元素的font-size × 你想要的像素值 = em值  

## REM
那么REM其实和EM没啥区别。
本质在REM以`<html>`标签中的`font-size`为依据