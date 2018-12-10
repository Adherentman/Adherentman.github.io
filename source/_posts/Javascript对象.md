---
title: JavaScript对象
date: 2017-11-30 23：23
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

# Javascript对象

JavaScript对象可以通过两种形式定义： 声明（文字）形式和构造形式

```javascript
//声明形式
var object = {
  a:1
};
//构造形式
var b = new Object();
b.key = 1;	//给b添加个属性

```
<!--more-->
通过这两种形式我发现一个大问题！

用构造函数去创建对象我们只能通过`.`去添加属性，那真是可太麻烦了。。所以我基本上都用声明形式去创建对象。



那么我想要得到`b`中`key`的值呢？？

```javascript
console.log(b.key,'.');
console.log(b["key"],'[]');
```

我们有两种方法可以做到哦！

1. 用`.`操作符通常被我们叫做“属性访问”；
2. 用`[]`操作符通常被我们叫做“键访问”；

但是我发现他们做的是同一件事情。。那我就把他们统称一下叫“属性访问”啦！

哦对啦！在ES6中还有个好玩的方法！

```javascript
var prev = "foo";

var myObject = {
  [prev + "Hello"] : "Hello",
  [prev + "World"] : "World",
}

console.log(myObject["prevHello"]); -> Hello
console.log(myObject["prevWorld"]); -> World
```

这是不是很神奇！我们可以通过`+`号实现了可计算的属性名。

多亏了ES6的`Symbol`,它是一种新的类型，在这我就不多说啦，贴上[mdn](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Symbol)。

## 属性描述符

![getOwnPropertyDescriptor](http://ozar6ogjb.bkt.clouddn.com/getOwnPropertyDescriptor.png)

> writable：可写的
>
> enumerable: 可枚举的
>
> configurable: 可配置的

那么如果我做以下操作呢？

```javascript
Object.defineProperty(myObject, "a",{
  value:2,
  writable:false,
  configurable:false,
  enumerable:false
})
```

![defineProperty](http://ozar6ogjb.bkt.clouddn.com/defineProperty.png)

也就是这个`myObject`对象变成了不可写、不可枚举、不可配置啦。

那么我可以通过这个特性去实现一个不可变（Immutable）的对象了！

```javascript
var zoo = {};
Object.defineProperty(myObject, "cat",{
  cat: "cat",
  writable:false,
  configurable:false,
})
```

但是我只想要我的动物园(`zoo`)里只有猫不想要别的小动物了！我只能用`Object.preventExtensions()`来禁止别的小动物进入我的动物园，而且还保留了猫。

```javascript
var zoo = {
  cat: "cat"
};
Object.Object.preventExtensions(zoo);

zoo.dog = "dog";
zoo.dog; ->//undefined
```

还有2种方法可以做到不可变(不详细讲解，附上mdn)8：

1. [Object.seal()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/seal)
2. [Object.freeze()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Object/freeze)



在JavaScript中，数组是对象，函数是对象，正则表达式是对象。那么对象自然也是对象。

- JavaScript中的对象是无类型的。
- 对象是属性的容器，其中每个属性都拥有名字和值。
- JavaScript包含一种原型链的特性，允许对象继承另一个对象的属性。

一张简略图
![简略](http://ozar6ogjb.bkt.clouddn.com/yuanxin.png)
![\__proto__与prototype](http://ozar6ogjb.bkt.clouddn.com/_proto_.jpg)

<!--more-->

- 对象最常见的用法
  - 创建（create）
  - 设置（set）
  - 查找（query）
  - 删除（delete）
  - 检测（test）
  - 枚举（enumerate）
- 每个属性还有一些与之相关的值，称为**属性特性**
  - 可写
  - 可枚举
  - 可配置
- 除了包含属性之外，每个对象还拥有三个相关的对象特性
  - 对象的原型(prototype)
  - 对象的类(class)
  - 对象的扩展标记
- 内置对象，如**数组，函数，日期，和正则表达式都是内置对象**
- 宿主对象，简单的理解就是BOM、DOM和自己定义的对象
- 自定义对象，就是我们自己创建的对象
- 自有属性，直接在对象中定义的属性
- 继承属性，在对象原型对象中定义的属性。

## new创建对象
`new` 运算符创建并初始化一个新对象。关键字new后跟随一个**函数**调用。
这个**函数**称做**构造函数**。

## 对象字面量

```JavaScript
var empty_project = {};
var stooge = {				//对象字面量
  "first-name": "Jerome",
  "last-name": "Howard"
};
```

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



## 属性的查询

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

## 属性的设置

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

## 关联数组的对象
`object["property"]`，这个看起来更像数组，但是这个数组元素是通过字符串索引。这种数组就是**关联数组**，别名“散列”，“映射”，“字典”。**JavaScript对象都是关联数组**。

## 引用

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

## 原型

每个对象都连接着另一个对象相关联，这个对象就是原型。而且每一个对象都可以从原型继承属性。

所有通过对象字面量创建的对象都连接到`Object.prototype`，它是JavaScript中的标配对象。

那么由new Date()创建的Date对象的属性同时继承自`Date.prototype`和`Object.prototype`。这一系列链接在一起的原型对象，就是我们所说的**原型链**。

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

### 原型连接在更新时是不起作用的

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

### 原型关系

我们添加一个新的属性到**原型**中，该属性会立即对**所有**基于该原型创建的对象可见。

```javascript
stooge.profession = 'actor';
another_stooge.profession  //'actor
```

## 反射

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

##属性的特性

数据属性的4个特性：

- 它的值
- 可写性
- 可枚举性
- 可配置性

存取器属性的4个特性：

- 读取
- 写入
- 可枚举性
- 可配置性

为了实现属性特性的查询和设置，我们就有了一个名为 **”属性描述符（property descriptor）”**。这个对象代表了那4个**特性**。

数据属性的描述符对象的属性有`value（它的值）`, `writable（可写性）`, `enumerable（可枚举性）`, `configurable（可配置性）`。

存取器属性的描述符对象的属性有`get属性`, `set属性`代替 `value（它的值）`, `writable（可写性）`, `enumerable（可枚举性）`,	`configurable（可配置性）`。

而且[`writable（可写性）`，`enumerable（可枚举性）`,`configurable（可配置性）`这仨都是布尔值]。

而且get属性和set属性是函数值。

我们通过调用`Object.getOwnPropertyDescriptor()`可以获得某个对象特定属性的属性描述符。

```javascript
Object.getOwnPropertyDescriptor({x:1},"x");
//返回{ value: 1, writable: true, enumerable: true, configurable: ture }
```

要想获得继承属性的特性，需要遍历原型链，`Object.getProtorypeOf()`

要想设置属性的特性，需要调用`Object.definePeoperty()`：

```javascript
var o = {};
//添加一个不可枚举的数据熟悉
Object.definePeoperty(o, "x", {
  value:1,
  writable:true,
  enumerable:false,
  configurable:true,
});
//属性是存在的，但是不可枚举
o.x; 						// => 1
Object.keys(o);				// => []

//对属性x做修改，让它变为只读
Object.defineProperty(o, "x",{writable: flase});

//试图去改
o.x = 2;				//操作失败，但是不报错，在严格模式下会抛出类型错误异常
o.x;					// => 1

Object.definePropetry(o, "x", {value:2});
o.x;					// => 2

Object.definePropetry(o, "x", {get: function(){return 0;}});
o.x;					// => 0
```

## 类属性

对象的类属性是一个**字符串**。

##序列化对象

对象序列化是指将对象的状态转换为字符串，也可将字符串还原为对象。

`JSON.stringify()`,`JSON.parse()`用来序列化和还原JavaScript对象。

## 对象方法

### toString()

```javascript
var s = {x:1, y:1}.toString();

// => [object, Object]
```

### toLocaleString()

这个方法返回一个表示这个对象的本地化字符串.

### toJSON()

Object.prototype实际上是没有定义toJSON()方法的，JSON.stringify()方法会调用toJSON()方法。

### valueOf()

这个方法和`toString()`非常类似。

当JavaScript需要将对象转换为某种原始值而非字符串的时候才会调用它，尤其是转换为数字的时候。

## 减少全局变量污染

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