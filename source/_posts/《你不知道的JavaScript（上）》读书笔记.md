---
title: 《你不知道的JavaScript（上）》读书笔记
date: 2017-12-23 21:40:30
comments: true
layout: post
tags: [读书笔记]
categories: 读书笔记
---

## This

在JS中This让我们捉摸不透。看了《你不知道的JavaScript（上）》让我对this有了更胜的认识。

总结一下：

This实际上是在函数被调用时发生的绑定，它指向什么完全取决于函数在哪里被调用。

<!--more-->

那么我们要去判断一个运行中函数的`this`绑定，我们需要先知道：

1. 函数在哪里被直接调用？
2. 找到之后，我们需要去通过四条顺序规则去判断`this`的绑定对象


### 四条顺序

 1. 是否是被`new`调用的？是的话那就绑定到新的创建对象。
    1. ```javascript
       var bar = new foo()
       ```

 2. 是否被`call`或者`apply`或者`bind`调用？是的话那就绑定到指定的对象。
    1. ```javascript
       var bar = call(obj, 2);
       ```

 3. 是否由上下文调用？是的话那就绑定到那个上下文对象。

 4. 默认：在默认情况下，如果我们在严格模式下则为`undefined`，否则我们会绑定到全局对象

## 对象

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
