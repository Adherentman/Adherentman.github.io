---
title: Sass小笔记
date: 2018-06-21 21:26
comments: true
layout: post
tags: [CSS]
categories: CSS
---

## 父选择器的标识符&
```scss
article a {
  color: blue;
  &:hover { color: red }
}
-> 
article a { color: blue }
article a:hover { color: red }
```

## 嵌套属性
```scss
nav {
  border: {
  style: solid;
  width: 1px;
  color: #ccc;
  }
}
->
nav {
  border-style: solid;
  border-width: 1px;
  border-color: #ccc;
}
```
<!--more-->
## @mixin
```scss
@mixin rounded-corners {
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
notice {
  background-color: green;
  border: 2px solid #00aa00;
  @include rounded-corners;
}
->
.notice {
  background-color: green;
  border: 2px solid #00aa00;
  -moz-border-radius: 5px;
  -webkit-border-radius: 5px;
  border-radius: 5px;
}
```
## @mixin传参
```scss
@mixin link-colors($normal, $hover, $visited) {
  color: $normal;
  &:hover { color: $hover; }
  &:visited { color: $visited; }
}
a {
  @include link-colors(blue, red, green);
}

//Sass最终生成的是：
a { color: blue; }
a:hover { color: red; }
a:visited { color: green; }
```
## 继承
```scss
//通过选择器继承继承样式
.error {
  border: 1px solid red;
  background-color: #fdd;
}
.seriousError {
  @extend .error;
  border-width: 3px;
}
```