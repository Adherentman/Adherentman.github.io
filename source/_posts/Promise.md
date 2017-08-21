---
title: Promise
date: 2017-07-28 20：37
comments: true
layout: post
tags: [JavaScript,node.js]
categories: Javascript修仙之路
---

# Promise

```javascript
new Promise(
/* executor */
   function(resolve,reject){...}
)
```

> executor 函数在Promise构造函数执行时同步执行，被传递resolve和reject函数（executor 函数在Promise构造函数返回新建对象前被调用）。

一个 `Promise`有以下几种状态:

- *pending*: 初始状态，不是成功或失败状态。
- *fulfilled*: 意味着操作成功完成。
- *rejected*: 意味着操作失败。


<!--more-->

# Promise对象

- Promise是一个构造函数


- 对象的状态不受外界影响

- 一旦状态改变就不会再变,任何时候都可以得到这个结果.

  > 状态改变只有两种可能:从`Pending` 变为`Resolved`(''未完成''变为''成功''将异步操作**成功**的结果作为参数传递出去)
  >
  > 从`Pending`变为`Rejected`("未完成"变为"失败"将异步操作报出**错误**的结果作为参数传递出去).


# Promise原型

## then()方法

### MDN的例子

```javascript
let p1 = new Promise(function(resolve, reject) {
  resolve("Success!");
  // or
  // reject ("Error!");
});

p1.then(function(value) {
  console.log(value); // Success!
}, function(reason) {
  console.log(reason); // Error!
});
```

![promise1](/images/promise1.png)

当我把`reject`去掉注释

![promise2](/images/promise2.png)

![promise3](/images/promise3.png)

说明`p1`已经被声明过了,而且状态改变过了就不会在改变了.

我只能把`p1` 改成别的才能得到`Error`.

## 链式

例子来自<ES6标准入门>

```javascript
getJSON("/post/1.json").then(function(post){
  return getJSON(post.commentURL);
}).then(function funcA(comments){
  consloe.log("Resolved:",comments);
},function funcB(err){
  consloe.log("Rejected:",err);
});
```

第一个then方法指定的回调函数返回的是另一个Promise对象.第二个then方法指定的回调函数会等待这个新的Promise对象状态发送变化再进行调用下面的`funcA`或者`funcB`函数.

## catch()方法

例子来自<ES6标准入门>

**catch()** 方法返回一个[Promise](https://developer.mozilla.org/zh-CN/docs/Web/API/Promise)，只处理拒绝的情况。它的行为与调用**then()**相同。

其实是 **.then(null,rejection)** 的别名.

```JavaScript
p.then((val) => console.log("fulfilled:", val))
	.catch((err) => console.log("rejected:", err));

//等同于

p.then((val) => console.log("fulfilled", val))
	.then(null,(err) => console.log("rejected:", val));
```

- Promise对象的错误具有"冒泡"性质,会一直向后传递,直到被捕获为止.也就是说,错误总是会被下一个catch语句捕获.

# Promise方法

## Promise.all()

**Promise.all(iterable)** 方法指当所有在可迭代参数中的 promises 已完成，或者第一个传递的 promise（指 reject）失败时，返回 promise。

```javascript
Promise.all(iterable);
```

> iterable
>
> 一个可迭代对象，例如 [`Array`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Array)。参见 [iterable](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/iterable).

来自[MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/all)的例子

```javascript
var p1 = Promise.resolve(3);
var p2 = 1337;
var p3 = new Promise((resolve, reject) => {
  setTimeout(resolve, 100, "foo");
}); 

Promise.all([p1, p2, p3]).then(values => { 
  console.log(values); // [3, 1337, "foo"] 
});
```

## [Promise.resolve(value)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/resolve)

## [Promise.reject(reason)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/reject)

## [Promise.race(iterable)](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Promise/race)

# 回调函数

一个回调函数，也被称为高阶函数，是一个被作为参数传递给另一个函数.

你到一个商店买东西，刚好你要的东西没有货，于是你在店员那里留下了你的电话，过了几天店里有货了，店员就打了你的电话，然后你接到电话后就到店里去取了货。在这个例子里，你的电话号码就叫回调函数，你把电话留给店员就叫登记回调函数，店里后来有货了叫做触发了回调关联的事件，店员给你打电话叫做调用回调函数，你到店里去取货叫做响应回调事件。回答完毕。 

作者：常溪玲链接：https://www.zhihu.com/question/19801131/answer/13005983来源：知乎著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。