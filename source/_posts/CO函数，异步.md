---
title: CO函数，异步
date: 2017-08-18 23：02
comments: true
layout: post
tags: [JavaScript,node.js]
categories: Javascript修仙之路
---

# CO函数库

[co 函数库](https://github.com/tj/co)是著名程序员 TJ Holowaychuk 于2013年6月发布的一个小工具，用于 Generator 函数的自动执行。

有一个 Generator 函数，用于依次读取两个文件。

> ```JavaScript
> var gen = function* (){
>   var f1 = yield readFile('/etc/fstab');
>   var f2 = yield readFile('/etc/shells');
>   console.log(f1.toString());
>   console.log(f2.toString());
> };
> ```

**co 函数库可以让你不用编写 Generator 函数的执行器。**

> ```JavaScript
> var co = require('co');
> co(gen);
> ```

上面代码中，Generator 函数只要传入 co 函数，就会自动执行。

co 函数返回一个 Promise 对象，因此可以用 then 方法添加回调函数。

> ```JavaScript
> co(gen).then(function (){
>   console.log('Generator 函数执行完成');
> })
> ```



## co 函数库的源码

co 就是上面那个自动执行器的扩展，它的[源码](https://github.com/tj/co/blob/master/index.js)只有几十行，非常简单。

首先，co 函数接受 Generator 函数作为参数，返回一个 Promise 对象。

> ```JavaScript
> function co(gen) {
>   var ctx = this;
>
>   return new Promise(function(resolve, reject) {
>   });
> }
> ```

在返回的 Promise 对象里面，co 先检查参数 gen 是否为 Generator 函数。如果是，就执行该函数，得到一个内部指针对象；如果不是就返回，并将 Promise 对象的状态改为 resolved 。

> ```JavaScript
> function co(gen) {
>   var ctx = this;
>
>   return new Promise(function(resolve, reject) {
>     if (typeof gen === 'function') gen = gen.call(ctx);
>     if (!gen || typeof gen.next !== 'function') return resolve(gen);
>   });
> }
> ```

接着，co 将 Generator 函数的内部指针对象的 next 方法，包装成 onFulefilled 函数。这主要是为了能够捕捉抛出的错误。

> ```JavaScript
> function co(gen) {
>   var ctx = this;
>
>   return new Promise(function(resolve, reject) {
>     if (typeof gen === 'function') gen = gen.call(ctx);
>     if (!gen || typeof gen.next !== 'function') return resolve(gen);
>
>     onFulfilled();
>     function onFulfilled(res) {
>       var ret;
>       try {
>         ret = gen.next(res);
>       } catch (e) {
>         return reject(e);
>       }
>       next(ret);
>     }    
>   });
> }
> ```

最后，就是关键的 next 函数，它会反复调用自身。

> ```JavaScript
> function next(ret) {
>   if (ret.done) return resolve(ret.value);
>   var value = toPromise.call(ctx, ret.value);
>   if (value && isPromise(value)) return value.then(onFulfilled, onRejected);
>   return onRejected(new TypeError('You may only yield a function, promise, generator, array, or object, '
>         + 'but the following object was passed: "' + String(ret.value) + '"'));
>     }
> });
> ```

上面代码中，next 函数的内部代码，一共只有四行命令。

> 第一行，检查当前是否为 Generator 函数的最后一步，如果是就返回。
>
> 第二行，确保每一步的返回值，是 Promise 对象。
>
> 第三行，使用 then 方法，为返回值加上回调函数，然后通过 onFulfilled 函数再次调用 next 函数。
>
> 第四行，在参数不符合要求的情况下（参数非 Thunk 函数和 Promise 对象），将 Promise 对象的状态改为 rejected，从而终止执行。

## 并发的异步操作

co 支持并发的异步操作，即允许某些操作同时进行，等到它们全部完成，才进行下一步。

这时，要把并发的操作都放在数组或对象里面。

> ```JavaScript
> co(function* (){
>   var values = [n1,n2,n3];
>   yield values.map(somethingAsync);
> });
> function* somethingAsync(x){
>   return y
> };
> ```

# 