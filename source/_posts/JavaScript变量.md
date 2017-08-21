---
title: JavaScript变量+方法
date: 2017-04-24 13:55
comments: true
layout: post
tags: [JavaScript]
categories: Javascript基础
---

# JavaScript变量+方法

ECMAScript变量可能包含两种不同数据类型的值：基本类型值和引用类型值。

**基本类型值**指的是简单的数据段，而**引用类型值**指那些可能有多个值构成的对象。

## 传递参数

```javascript
function addTen(num){
  num +=10;
  return num;
}
var count =20;
var result = addTen(count);
alert(count); 		//20,没变化
alert(result);		//30
```

在这上面看了高程三的话懵懵懂懂，实际上在说的是，要执行`alert(count);`时`count`只是把20复制给参数num，以便20在addTen()中使用，在函数里面num的值确实+10变成了30，但是不影响外部count的变量。

<!--more-->

## 方法

- **split()**

方法讲一个字符串对象的每个字符拆出来，并且将每个字符串当数组的每个元素。

## 重排序方法

- **reverse()和sort()**

**reverse()**方法会反转数组项的顺序。

```javascript
var values = [1,2,3,4,5];
values.reverse();
alert(values); 			//5,4,3,2,1
```

而 **sort()**方法是按升序排列数组项。——最小的值位于前面，最大的值排在最后面。而且`sort()`方法会调用每个数组项的`toString()`转型方法，然后去比较得到的字符串。

```javascript
var values = [0,1,5,10,15];
values.sort();
alert(values); 		//0,1,10,15,5
```

在字符串的比较时，10则位于5的前面，于是数组的顺序就被修改了。当然改为降序也行参考注释

```javascript
function compare (value1,value2){
  if(value1 <value2){
    return -1; 			//return 1;
  }else if(value1 >value2){
    return 1;			//return -1;
  }else{
    return 0;
  }
}
```

然后我们再把这个参数传给`sort()`方法即可。

```javascript
var values = [0,1,5,10,15];
values.sort(compare);
alert(values); 		//0,1,5,10,15
```

