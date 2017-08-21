---
title: JavaScript——16道算法题
date: 2017-05-15 13：07
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之路
---

# Let‘s go

#### 1.Reverse a String 

翻转字符串

先把字符串转化成数组，再借助数组的reverse方法翻转数组顺序，最后把数组转化成字符串。

你的结果必须得是一个字符串

这是一些对你有帮助的资源:

- [Global String Object](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String)


- [String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)


- [Array.reverse()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/reverse)


- [Array.join()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Array/join)

> `reverseString("hello")` 应该返回 `"olleh"`.

```javascript
function reverseString(str) {
  return str.split('').reverse().join('');
}
reverseString("hello");

```

<!-- more -->

#### 2.Factorialize a Number

计算一个整数的阶乘

如果用字母n来代表一个整数，阶乘代表着所有小于或等于n的整数的乘积。

阶乘通常简写成 `n!`

例如: `5! = 1 * 2 * 3 * 4 * 5 = 120`

这是一些对你有帮助的资源:

- [Arithmetic Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Arithmetic_Operators)

> `factorialize(5)` 应该返回 120.

```javascript
function factorialize(num) {
  if(num<0){
    return -1; 
  }else if(num ===0||num===1){
    return 1;
  }else{
  return (num*factorialize(num-1));
  }
}
factorialize(5);

```

#### 3.Check for Palindromes 

如果给定的字符串是回文，返回`true`，反之，返回`false`。

如果一个字符串忽略标点符号、大小写和空格，正着读和反着读一模一样，那么这个字符串就是palindrome(回文)。

**注意**你需要去掉字符串多余的标点符号和空格，然后把字符串转化成小写来验证此字符串是否为回文。

函数参数的值可以为`"racecar"`，`"RaceCar"`和`"race CAR"`。

这是一些对你有帮助的资源:

- [String.replace()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/replace)


- [String.toLowerCase()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/toLowerCase)

> `palindrome("race car")` 应该返回 true.
>
> `palindrome("not a palindrome")` 应该返回 false.

```javascript
function palindrome(str) {
  // Good luck!
  var re = /[\W_]/g;
  var slo=str.toLowerCase().replace(re,"");
  var slow=slo.split('').reverse().join('');
  if(slo==slow){
  return true;}
  else{
    return false;
  }
}
palindrome("我爱你");

```

#### 4.Find the Longest Word in a String 

找到提供的句子中最长的单词，并计算它的长度。

函数的返回值应该是一个数字。

这是一些对你有帮助的资源:

- [String.split()](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)


- [String.length](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/length)

> `findLongestWord("The quick brown fox jumped over the lazy dog")` 应该返回 6.

```javascript
function findLongestWord(str) {
  var arr=str.split(' ');
  var long=0;
  for(var i=0;i<arr.length;i++){
    if(arr[i].length>long){
      long=arr[i].length;
    }
  }
  return long;
}

findLongestWord("The quick brown fox jumped over the lazy dog");

```



#### 5.Title Case a Sentence

确保字符串的每个单词首字母都大写，其余部分小写。

像'the'和'of'这样的连接符同理。

这是一些对你有帮助的资源:

[String.split](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/String/split)

> `titleCase("I'm a little tea pot")` 应该返回 "I'm A Little Tea Pot".

```javascript
function titleCase(str) {
  var arr=str.toLowerCase().split(' ');
  var l=[];
  for(var i=0;i<arr.length;i++){
    var str1=arr[i].slice(0,1).toUpperCase()+arr[i].slice(1);
    l.push(str1);
  }
  return l.join(' ');
}

titleCase("I'm a little tea pot");
```

- 首先我们需要把这个字符串的每一项都变成小写字母并转换成数组
- 创建一个保存新的空数组
- 用遍历数组的长度。之后我们把每一项数组的第一个字母用`slice()`变成大写。
- 再`push`到空的数组里。
- 最后用`join()`数组返回字符串。

#### 6.Return Largest Numbers in Arrays

右边大数组中包含了4个小数组，分别找到每个小数组中的最大值，然后把它们串联起来，形成一个新数组。

提示：你可以用for循环来迭代数组，并通过`arr[i]`的方式来访问数组的每个元素。

这是一些对你有帮助的资源:

- [Comparison Operators](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/Comparison_Operators)

> `largestOfFour([[13, 27, 18, 26], [4, 5, 1, 3], [32, 35, 37, 39], [1000, 1001, 857, 1]])` 应该返回 `[27,5,39,1001]`.

**第一种方法**
```javascript
function largestOfFour(arr) {
  // You can do this!
 var l=[];
  for(var i=0;i<arr.length;i++){
    arr[i].sort(function(a,b){return b-a});
  }
  l.push(arr[i][0]);
}
return l;

largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```

**第二种方法**

```javascript
function largestOfFour(arr) {
  // You can do this!
  var temp = [];
  for(var i = 0; i < arr.length; i++){
    var l = arr[i].reduce(function(prev,cur,index,array){
      return prev > cur ? prev : cur;
    });
    temp.push(l);
  }
  return temp;
}
largestOfFour([[4, 5, 1, 3], [13, 27, 18, 26], [32, 35, 37, 39], [1000, 1001, 857, 1]]);
```