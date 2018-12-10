---
title: Generator函数
date: 2017-07-29 14：27
comments: true
layout: post
tags: [JavaScript,node.js]
categories: Javascript修仙之旅
---



Generator最大的特点就是定义的函数可以被**暂停执行**.

## 作用

迭代器 iterator, infinite range, 可以暂停函数, lazy evaluation, 用来实现 async/await 啊, 棒棒哒.

async generator/iterator 

> 摘自MDN

**生成器**对象是由一个 [generator function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Statements/function*) 返回的,并且它符合[可迭代协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#iterable)和[迭代器协议](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Iteration_protocols#iterator)。

> 摘自ES6标准入门

可以把它理解成一个状态机,封装了多个内部状态.还是一个遍历器对象生成函数.

<!--more-->

```javascript
function* hi(){
yield 'nihao';
yield 'hello';
return 'ending';
}
var hw = hi();
hw.next();
hw.next();
hw.next();
```

![generator](/images/generator.png)



> 通过gen.next()取得的输出是一个对象，包含value和done两个属性，其中value是真正返回的值，而done则用来标识Generator是否已经执行完毕。因为自然数生成器是一个无限循环，所以不存在done: true的情况。



在`Generator函数`返回的遍历器对象只有调用`next方法`才会遍历下一个内部状态,所以yield语句就是**暂停标志**.

- yield语句后面的表达式,只有当调用到next方法,内部指针指向该语句时才会执行.

```JavaScript
function* add(){
  yield 123+123;
}
//在上面代码里123+123不去求值.当有next();时,才去求值
```

1. 每个yield将代码分割成两个部分，需要执行两次next才能执行完。
2. yield其实由两个动作组成，**输入** + **输出**（输入在输出前面），每次执行next，代码会暂停在yield **输出**执行后，其它的语句不再执行（**很重要**）。

## for...of循环

可以自动遍历generator函数,不用去调用next方法.

```javascript
function* foo(){
  yiled 1;
  yiled 2;
  yiled 3;
  yiled 4;
  yiled 5;
  return 6;
}
for(let v of foo()){
  console.log(v);
}
//1 2 3 4 5
```

# Generator函数的数据交换和错误处理

`next()`方法返回值的`value`属性，是`Generator`函数向外输出的数据；`next()`方法还可以接受参数，向`Generator`函数体内输入数据。

```JavaScript
function* gen(x) {
    var y = yield x + 2;
    return y;
} 

var g = gen(1);
g.next()      // { value: 3, done: false }
g.next(2)     // { value: 2, done: true }
```

`Generator`函数内部还可以部署错误处理代码，捕获函数体外抛出的错误。

```
function* gen(x) {
    try {
        var y = yield x + 2
    } catch(e) {
        console.log(e)
    }
    return y
}

var g = gen(1);
g.next();
g.throw('出错了');
```

上面代码的最后一行，`Generator`函数体外，使用指针对象的`throw`方法抛出的错误，可以被函数体内的`try...catch` 代码块捕获。这意味着，出错的代码与处理错误的代码，实现了时间和空间上的分离，这对于异步编程无疑是很重要的。