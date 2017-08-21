---
title: JavaScript闭包
date: 2017-05-26 17：47
comments: true
layout: post
tags: [JavaScript]
categories: Javascript基础
---

# 闭包/bibao/Closures

## 什么是闭包

MDN对闭包的定义为：

> 闭包是指那些能够访问自由变量的函数。

阮老师对闭包的定义为：

>  闭包就是能够读取其他函数内部变量的函数。
>
>  可以把闭包简单理解成"定义在一个函数内部的函数"。

红宝书对闭包的定义为：

> 　闭包是指有权访问另一个函数作用域中的变量的函数

那么圣经犀牛书对闭包定义为：

> 函数对象可以通过作用域链相互关联起来，函数体内部的变量都可以保存在函数作用域内，这种特性在计算机科学文献中称为‘闭包’”

<!--more-->

**我个人比较认同红宝书的定义。**

```javascript
function foo(){
    var a = 2;
    function bar(){
        console.log(a); // 2
    }
    bar();
}
foo();
```

我们来做一个数组求和

```javascript
function sum(arr){
  return arr.reduce(function(x,y){
    return x+y;
  });
}
sum([3,4,5,6]);			//18
```

但是我们想要返回函数

```javascript
function smallsum(arr){
  var sum = function(){
    return arr.reduce(function(x,y){
      return x+y;
    });
  }
  return sum;
}
```

当我们想要用`smallsum`的时候返回的却是个函数。

```javascript
var result = smallsum([3,4,5,6]);		//function sum()
```

直到我们调用`result`

```javascript
result();			//18
```

在这个例子中，我发现内部函数`sum`可以调用外部函数`smallsum`的参数和局部变量。

当我们调用`smallsum`的时候，每次调用都会产生一个新的函数。即使你传入的值相同。

```JavaScript
var result1 = smallsum([3,4,5,6]);
var result2 = smallsum([3,4,5,6]);
result1 === result2; 			//false
```

## 假如

```javascript
function l(){
  var arr= [];
  for(var i=0;i<4;i++){
    arr.push(function(){
      return i;
    });
  }
  return arr;
}
```

```javascript
var a= l();
var f1=a[0];
var f2=a[1];
var f3=a[2];

f1();	//4
f2();	//4
f3();	//4
```

```javascript
function l(){
  var arr= [];
  for(var i=0;i<4;i++){
    arr.push((function(n){
      return function(){
      return n;
      }
    })(i));
  }
  return arr;
}
var a= l();
var f1=a[0];
var f2=a[1];
var f3=a[2];

f1();		//0
f2();		//1
f3();		//2
```

注意这里用了一个“创建一个匿名函数并立刻执行”的语法：

```javascript
(function (n) {
    return n;
})(1); //1
```