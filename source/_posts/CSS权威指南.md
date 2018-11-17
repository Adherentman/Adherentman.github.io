---
title: 《Css权威指南》
date: 2018-05-28 20:18
comments: true
layout: post
tags: 读书笔记
categories: 读书笔记
---

# CSS权威指南
## 候选样式表
```html
<link rel="alternate stylesheet" type="text/css" href="styles.css" title="Big Text"/>
```
<!--more-->
候选样式表在大多数`Gecko`的浏览器中得到支持，如：Firefox、Netscape以及Opera 7.
或者设置同样的`title`属性，之后设置不同的`media`属性，在各种场景会使用对应的`css`文件
## @import
@import也可以应用媒体。
```html
@import url(styles.css) all;
@import url(styles1.css) print;
@import url(styles2.css) print, screen;
```