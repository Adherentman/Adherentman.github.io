---
title: CSS布局
date: 2018-04-01 14:50
comments: true
layout: post
tags: [CSS]
categories: CSS
---

## 两栏——左固定右自适应布局

## 方法一

```html
<div class="left"></div>
<div class="right"></div>

// css 
.left {
  width: 40px;
  height: 200px;
  background: red;
  float: left
}

.right {
  height: 200px;
  margin-left: 40px;
  background: blue;
}
```

[Demo](https://codepan.net/gist/9c5dd595475bf4a68adc87c895f6e561)

<!--more-->

## 方法二

```Html
<div class="left"></div>
<div class="right"></div>

//css
.left {
  position: absolute;
  height: 200px;
  width: 40px;
  background: red;
}
.right {
  height: 200px;
  margin-left: 40px;
  background: blue;
}
```

[Demo](https://codepan.net/gist/4166718a79e3d3013d43a63bb4220b98)