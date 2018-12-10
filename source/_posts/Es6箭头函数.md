---
title: ES6箭头函数
date: 2017-04-27 15:29
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---
简单地说，

```javascript
var a = x => x;
//
var a =function(x){
  return x;
};
```

这个就是最简单的箭头函数。

<!--more-->

接着如果箭头函数不需要参数或者多个参数的话，

```JavaScript
var a = () => 1;
//
var a =function(){
  return 1;
};
或者
var a = _ =>1;
```

```javascript
var sum = (num1,num2) =>num1 +num2;
//
var sum =function(num1,num2){
  return num1+num2;
};
```

**箭头函数没有自己的`this`**，

```javascript
(function (){
  return [
    (()=> this.x).bind({x :'inner'})()
  ]
}).call({x :'outer'});
//['outer']
```

所以`bind`的方法无效。并且`call()`,`apply()`,也无效。

**MDN上的**

```javascript
var adder = {
  base : 1,
    
  add : function(a) {
    var f = v => v + this.base;
    return f(a);
  },

  addThruCall: function(a) {
    var f = v => v + this.base;
    var b = {
      base : 2
    };
            
    return f.call(b, a);
  }
};

console.log(adder.add(1));         // 输出 2
console.log(adder.addThruCall(1)); // 仍然输出 2（而不是3 ——译者注）
```







