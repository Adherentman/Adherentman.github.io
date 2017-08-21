---
title: JavaScript对象
date: 2017-08-02 23：00
comments: true
layout: post
tags: [JavaScript]
categories: Javascript基础
---

# Javascript对象

在JavaScript中，数组是对象，函数是对象，正则表达式是对象。那么对象自然也是对象。

- JavaScript中的对象是无类型的。
- 对象是属性的容器，其中每个属性都拥有名字和值。
- JavaScript包含一种原型链的特性，允许对象继承另一个对象的属性。

# 对象字面量

```JavaScript
var empty_project = {};
var stooge = {				//对象字面量
  "first-name": "Jerome",
  "last-name": "Howard"
};
```

<!--more-->

一个对象字面量就是包围在一对花括号中的零或多个“名/值”对。

```javascript
var flight = {
  airline: "Oceanic",
  number: 815,
  departure: {
    IATA: "SYD",
    time: "2014-09-22 14:55",
    city: "Sydney"
  },
  arrival:{
    IATA: "LAX",
    time: "2004-09-23 10:42",
    city: "Los Angeles"
  }
};
```

属性的值可以从包括另一个对象字面量在内的任意表达式中获得。对象是可以嵌套的。

# 检索

```javascript
stooge["first-name"] //Jerome
flight.departure.IATA //SYD
```

需要检索对象里包含的值，可以采用[ ]后缀中括住一个字符串表达式的方式。

但是最好用`.`表示法。因为它可读性好。

我们去检索不存在的：

```JavaScript
stooge["middle-name"]  //undefined
flight.status          //undefined
```

||运算符可以用来填充默认值：

```javascript
var middle = stooge["middle-name"] || "(none)";
var status = flight.status || "unkown";
```

如果我们从undefined的成员属性中取值会导致`TypeError`异常。这时候我们可以通过 `&&` 运算符来避免错误。

```Javascript
flight.equipment 					//undefined
flight.equipment.model				//throw "TypeError"
flight.equipment && flight.equipment.model 	//undefined
```

# 更新

对象里的值可以通过赋值语句来更新。如果属性吗已经存在于对象里，那么这个属性的值会被替换。

```JavaScript
stooge['first-name'] = 'Jerome';
```



```javascript
stooge['middle-name'] = 'Lester';
stooge.nickname = 'Curly';
flight.equipment = {
  model: 'Boeing 777'
};
flight.status = 'overdue';
```

那么这些属性全部会扩充到对象中。

# 引用

对象通过引用来传递，他们永远不会被**复制**

```javascript
var x = hi;
x.hello = 'what';
var how = hi.hello;
	// how为what。
//因为x和hi是指向同一个对象的引用。

var a = {},b = {},c = {};
//a,b,c每个都引用一个不同的对象
a = b = c = {};
//a,b,c都是引用同一个空对象
```

# 原型

每个对象都连接一个原型对象，并且可以从中继承属性。

所有通过对象字面量创建的对象都连接到`Object.prototype`，它是JavaScript中的标配对象。

```javascript
if (typeof Object.beget !== 'function'){
  Object.create = function (o){
    var F = function (){};
    F.prototype = o;
    return new F();
  };
}
var another_stooge = Object.create(stooge);
```

Object增加一个create方法，这个方法创建一个使用原对象作为其原型的新对象。

## 原型连接在更新时是不起作用的

```javascript
another_stooge['first-name'] = 'Harry';
another_stooge['middle-name'] = 'Moses';
another_stooge.nickname = 'Moe';
```

> 原型连接只有在检索值得时候才被用到。
>
> 如果我们尝试去获取对象的某个属性值，但对象没有此属性名。
>
> JavaScript会从原型对象中获取属性值  ——> 原型对象中没，就回去它原型中寻找 ——>直到最后到达终点Object.prototype。
>
> 假如想要的属性不存在于原型链，那么结果就只能是undefined。
>
> 以上的过程为委托。

## 原型关系

我们添加一个新的属性到**原型**中，该属性会立即对**所有**基于该原型创建的对象可见。

```javascript
stooge.profession = 'actor';
another_stooge.profession  //'actor
```

# 反射

typeof操作符对确定属性的类型很有帮助。

```javascript
typeof flight.number	//number
typeof flight.status 	//string
typeof flight.arrival 	//object
typeof flight.manifest 	//undefined
```



原型链中的任何值都会产生值

```javascript
typeof flight.toString		//function
typeof flight.constructor 	//function
```



有两种方法去处理掉这些不需要的属性。

- 第一个是让你的程序做检查并丢弃为函数的属性。
- 另一个方法是`hasOwnProperty` 方法，如果对象拥有独有的属性，它将返回true。不会检查原型链

```JavaScript
flight.hasOwnProperty('number')			//true
flight.hasOwnProperty('constructor')	//true
```



# 减少全局变量污染

最小化使用全局变量的方法之一是为你的应用只创建一个**唯一**的全局变量！！

```javascript
var MYAPP = {}; 		//该变量此时变成你的应用的容器

MYAPP.stooge = {
  "first-name": "Joe",
  "last-name": "Howard"
};
MYAPP.flight = {
  airline: "Oceanic",
  number: 815,
  departure: {
    IATA: "SYD",
    time: "2004-09-22 14:55",
    city: "Sydney"
  },
  arrival: {
    IATA: "LAX",
    time: "2004-09-23 21:59",
    city: "Los Angeles"
  }
};
```

只要把全局性的资源都纳入一个名称空间之下，你的**程序与其他应用程序、组件、类库直接发送冲突的可能性就会显著降低。**

因为`MYAPP.stooge`指向的是顶层结构。