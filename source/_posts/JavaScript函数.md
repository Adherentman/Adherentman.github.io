---
title: JavaScript函数
date: 2017-08-16 22:45
updated: 2018-04-27 15:19:40
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---



在JavaScript中，函数是**一等公民**。函数是**第一型对象**。

所以说，我们可以将其视为其他任意类型的JavaScript对象。

<!--more-->

在JavaScript中函数可以：

- 可以赋值给**变量，数组，或其他对象**的属性
- 可以通过**字面量**进行创建
- 将其作为**参数**进行传递
- 可以作为函数的**返回值**进行返回
- 可以拥有**动态创建并赋值**的属性

最重要是的，它们还可以被**调用**。这些调用通常是以**异步方式**进行调用。

# 回调

回调函数的术语源于：我们定义一个函数，以便其他一些代码在适当的时机回头再调用他。

```javascript
function useless(callback){
    return callback();
}
```



```Javascript
var text = 'Demo arigato';
assert(useless(function(){
    return text;
}) ===text, "This useless function works!" + text);
//assert是测试函数

function assert(value, desc){
  var li = document.createElement("li");
  li.className = value ? "pase" : "fail";
  li.appendChild(document.createTextNode(desc));
  document.getElementById("results").appendChild(li);
};
```

# 函数字面量

函数字面量由4个部分组成

- function关键字
- 可选名称。
- 括号内部，一个以逗号分隔的参数列表。
- 包含在大括号内的一系列JavaScript语句叫 **函数体**。

注意：

> 所有的函数都有name属性，该属性保存的是他们的名称的字符串。
>
> 当然没有名称的函数也有name属性，只是为空字符串。

```javascript
var canFly = function() {
    return true;
};
```

这个函数我们可以通过它的引用 **canFly** 进行调用。它与canFly函数几乎一模一样，但是不一样的地方在于它的字符串值为” ”，而不是“canFly”。

```javascript
window.isDeadly = function() {
  return true;
};
```

我们可以（ **window.isDeadly() || isDeadly()** ）去调用这个函数，其实这就跟命名函数几乎一模一样了。

# 函数调用

4个不同的方式可以进行函数调用。

1. 作为一个函数进行调用
2. 作为一个方法进行调用，在对象是进行调用，支持面向对象编程
3. 作为构造器进行调用，创建一个新对象
4. 通过`apply()`或`call()`方法进行调用。

## 函数传递两个隐式参数

**arguments和this**

隐式（limplicit），意味着这些参数不会显示列在函数签名里。但是他们都会默默的传递给函数并存在于函数作用于内。

### arguments

它是传递给函数的所有参数的一个集合，它有一个`length`属性。比如：

`arguments[2]`表示获取第三个参数。

虽然它可以使用数组进行获取，甚至可以用for循环对它进行遍历。但是它确实不是JS数组。

我们只要将它看成一个类数组结构，并且 **只拥有数组** 的某些特性，仅此而已。

### this

this参数引用了与该函数调用进行隐式关联的一个对象，被称为**函数上下文。**

## 作为函数进行调用

很简单，比如说：

```javascript
function hi(){
    return hello;
}
hi();

var xzh = function(){
    return xuzihao;
}
xzh();
```

这就是作为函数调用。这种方法调用函数的上下文就是-------window对象。

## 作为方法进行调用

```javascript
var o = {};
o.whatever = function (){};
o.whatever();
```

那么`o.whatever`的函数上下文为**o**。

## 作为构造器进行调用

```javascript
function creep() {
    return this;
}
new creep();
```

### 构造器的超能力

构造器调用时，下面的特殊行为会发生：

- 创建一个新的空对象
- 传递给构造器的对象是this参数，从而成为构造器的函数上下文。
- 如果没有显式的返回值，新创建的对象则作为构造器的返回值进行返回。

## 使用apply()和call()方法进行调用

可以显式指定任何一个对象作为其函数上下文。

JavaScript的每个函数都有**apply()** 和 **call()**方法。使用其中一个我们都可以实现这种功能。

```javascript
function juggle(){
    var result = 0;
  	for(var n = 0; n < arguments.length; n++){
        result += arguments[n];
    }
  	this.result = result;
}

var ninja1 = {};
var ninja2 = {};

//apply() 传入2个参数： 一个是作为函数上下文的对象，另外一个是作为函数参数所组成的数组。
juggle.apply(ninja1, [1,2,3,4]);
//call() 传入两个参数： 一个是作为函数上下文的对象，另外一个是一个参数列表。
juggle.call(ninja2, 5,6,7,8);

//assert是断言测试用的
assert(ninja1.result === 10, "juggled via apply");
assert(ninja2.result === 26, "juggled via call");

```

## 五种this的工作原理

### 全局

> this;

this指向全局对象，浏览器：window，js:global

### 函数调用

> foo();

this也指向全局对象

### 方法调用

> bar.foo();

this指向被调用的对象。

调用构造函数

> new foo();

this指向新创建的对像。

### 显式的设置this

> apply, call

当使用 `Function.prototype` 上的 `call` 或者 `apply` 方法时，函数内的 `this` 将会被 **显式设置**为函数调用的第一个参数。

## 总结

- 作为函数进行调用：该上下文是方法的拥有者。
- 作为全局函数进行调用：该上下文永远是windos。
- 作为构造器函数进行调用：该上下文对象则是新创建的对象实例。
- 通过函数的apply（）或call（）方法进行调用时，上下文可以设置成任意值。