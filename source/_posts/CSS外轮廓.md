---
title: CSS3外轮廓
date: 2018-03-18 22:38
comments: true
layout: post
tags: [CSS]
categories: CSS
---

# Outline

看一个Outline的例子：[outline](https://codepan.net/gist/7a96f5e15ee26e3523fd17c577c01c61)

在下面看见一个div也有类似的效果。但是我们打开开发者工具看会发现。下面用`outline`实现的外边框中border为`-`。

## outline和border对比

* border属于盒模型的一部分，会直接影响盒子大小。
* outline则是将边框画在盒子上，不会影响该框。所以它不会破坏网页布局。
* border创建是可以显示单边的，而outline是不行的。看demo：[单边](https://codepan.net/gist/92abb7960aed0c5917f235f1a6180e5c)
* border只可以外轮廓，而outline可以借助`outline-offset`设成负值就可以实现内轮廓，而设成正值就是外轮廓。看demo：[内轮廓](https://codepan.net/gist/de786dc265e45ebefcd6a54614b348c4)