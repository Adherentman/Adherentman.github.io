---
title: JavaScript作用域与作用域链
date: 2017-05-18 17：37
comments: true
layout: post
tags: [JavaScript]
categories: Javascript基础
---

# Javascript作用域与作用域链

## 作用域

简单的说，作用域就是变量与函数的可访问范围，即作用域控制着变量与函数的可见性和生命周期。在JavaScript中，变量的作用域有全局作用域和局部作用域两种。

### 变量作用域

##### 全局变量拥有全局作用域。在JavaScript任何地方都是有定义的。

```javascript
var l="我是全局变量";			//声明了一个全局变量
function scope(){		
  var l="我是局部变量";		//声明了一个同名的局部变量
  return l;
}
scope()					//输出“我是局部变量”
```

##### <!--more -->

在函数内声明变量前不加`var`就是一个全局变量。

```javascript
var l="我是全局变量";
function scope(){
 	l="我还是全局变量";			//改变了全局变量
	m="我是一个新的全局变量";	  	 //声明了一个新的全局变量
	return [l,m];
}
scope()						   //输出"我是一个新的全局变量"
l
m
```

##### 在函数内声明的变量只在函数内有定义，它们是局部变量，作用域是局部性的。

```javascript
var l="我是全局变量";
function scope(){
  var l="我是局部变量";
  function scope1(){
    var l="我是一个新的局部变量";			//嵌套作用域内的局部变量
    return l;						  //返回当前作用域内的值
  }
  return scope1();
}
scope();							 //嵌套作用域

```

### 函数作用域和声明提前

函数作用域是指在函数内声明的所有变量在函数体内始终是可见的。只是改变函数内部。

变量在声明它们的函数体以及这个函数体嵌套的任意的函数体内都是有定义的。

```javascript
function func() {
            console.log(num);           //输出：undefined，而非报错，因为变量num在整个函数体内都是有定义的
            var num = 1;                //声明num 在整个函数体func内都有定义
            console.log(num);           //输出：1
        }
        func();
```

再看看下面这个例子：

```javascript
var num=2;
function func() {
            var num;
            console.log(num);           //输出：undefined，而非报错，因为变量num在整个函数体内都是有定义的
            var num = 1;                //声明num 在整个函数体func内都有定义
            console.log(num);           //输出：1
        }
        console.log(num);				//输出2
        func();
```

## 作用域链

**用途：**

保证对**执行环境有权访问**的**所有变量和函数的有序访问**。

作用域链上有两个对象。

- 第一个是定义函数参数和局部变量的对象
- 第二个是全局对象

当定义一个函数时，它实际上保存一个作用域链。

**高程三下这个例子就特别好**

```javascript
var color = "blue";

function changeColor(){
  var anotherColor = "red";
  
  function swapColors(){
    var tempColor = anotherColor;
    anotherColor = color;
    color = tempColor;
    //能访问 color、anotherColor、tempColor。
  }
  swapColors();			//能访问color、anotherColor不能访问tempColor。
}
changeColor();			//只能访问color
```

`changeColor()`的作用域链中只包含2个对象：它自己的变量对象和全局变量对象。所以它不能访问`swapColor()`的环境。

那么在`swapColors()`的作用域链中又3个对象：`swapColors()`的变量对象、`changeColor()`的变量对象和全局变量对象。