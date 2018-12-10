---
title: 《高性能JavaScript》读书笔记
date: 2018-04-02 14:39
comments: true
layout: post
tags: 读书笔记
categories: 读书笔记
---

## First

`<script>`方面的我写了一篇博文链接地址：[浅谈script标签](http://xuzihao.fun/%E6%B5%85%E8%B0%88script%E6%A0%87%E7%AD%BE.html)

## Second

数据存储：

1. 把数据存在访问字面量和局部变量是然后去访问速度最快的。
2. 全局变量总是在作用域的最末端所以访问速度也是最慢的。
3. 对象的深度嵌套拿值会影响性能。

<!--more-->

例子：

```javascript
//before

function b(){
    var getId = document.getElementById("id");
    var getClass = documnent.getElementsByClassName("class");
    getId.style.width......
}
    
//after
    function a(){
        var doc = document;
        var getId = doc.getElementById("id");
        var getClass = doc.getElementsByClassName("class");
        ....
    }
```

