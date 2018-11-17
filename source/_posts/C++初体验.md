---
title: C++ 初体验
date: 2017-12-08 22:05
updated: 2017-12-09 14:27:28
comments: true
thumbnail: http://ozar6ogjb.bkt.clouddn.com/c++.jpeg
layout: post
tags: [C++]
categories: C++
---

## First

Clion给我建的第一个C++文件如下
```c++
#include <iostream>

int main() {
    std::cout << "Hello, World!" << std::endl;
    return 0;
}
```
<!--more-->
那么我在想这和平常写得js不同啊，那个`std::`是啥...

经过强大的搜索引擎我知道了！这玩意儿就是一个类（std）啊（输入输出标准），它包括了cin成员和cout成员。

当我加了`using namespace std;`以后，我才能使用它的成员。我的c++程序可以写成:

```c++
#include <iostream>
using namespace std;
 
// main() 是程序开始执行的地方
 
int main()
{
   cout << "Hello World"; // 输出 Hello World
   return 0;
}
```

那我又发现 `namespace`这个东西。原来它是指标识符的各种可见范围。



## C++数据类型

讲到数据类型。。我这个写惯了动态语言的人。。。诶西。强制让我写静态有点不行惯，没办法只能去写啊！

C++有七种数据类型：

| 类型   | 关键字     |
| ---- | ------- |
| 布尔型  | bool    |
| 字符型  | Char    |
| 整型   | int     |
| 浮点型  | float   |
| 双浮点型 | double  |
| 无类型  | void    |
| 宽字符型 | wchar_t |

有一些基本类型可以使用一个或多个类型装饰符进行修饰：

* signed
* unsigned
* short
* long



## 枚举类型

枚举大家可能都听过吧，它其实就是将变量的值一一列举出来，在这提醒下！变量的值只能在列举出来的值的范围内。

```c++
enum animal { dog, cat, pig } c;
c = cat;
//这段代码意思为 变量c的类型为animal，最后c被赋值给cat。
```

在默认情况下第一个名称的值为0，第二个名称值为1，第三个名称的值为2.

如果我这有写：

```c++
enum animal { dog, cat=5, pig };
```

在这里，cat的值为5，那么pig值为6.



## 变量类型

类型决定了变量存储的大小和布局，该范围内的值都可以存储在内存中，运算符可应用于变量上。

* 布尔
* char
  * 通常为8位字节，这是一个整数类型
* int
* float
  * 单精度浮点类型，32位
* double
  * 双精度浮点类型，64位
* void
* wchar_t

而且C++ 也允许定义各种其他类型的变量，比如**枚举、指针、数组、引用、数据结构、类**。

我们可以使用 **extern** 关键字在任何地方声明一个变量

```c++
#include <iostream>
using namespace std;

extern int a, b;

int main(){
    int a, b;
  
  	a = 10;
  
  cout << a ;  //=> 10
}
```

## 作用域

在JS中我们的作用域可是太坑了！现在我们来看看C++的作用域吧。

- （1）在函数或一个代码块内部声明的变量，称为局部变量。
- （2）在函数参数的定义中声明的变量，称为形式参数。
- （3）在所有函数外部声明的变量，称为全局变量。

```c++
(1局部变量)
#include <iostream>
using namespace std;
 
int main ()
{
  // 局部变量声明
  int a, b;
  int c;
 
  // 实际初始化
  a = 10;
  b = 20;
  c = a + b;
 
  cout << c;
 
  return 0;
}
```



```c++
(3全局变量)
#include <iostream>
using namespace std;
 
// 全局变量声明
int g;
 
int main ()
{
  // 局部变量声明
  int a, b;
 
  // 实际初始化
  a = 10;
  b = 20;
  g = a + b;
 
  cout << g;
 
  return 0;
}
```

当我们遇到全局变量和局部变量的名称相同时，和js一样，**在函数体内局部变量的值会覆盖全局变量**。

## 定义常量

在 C++ 中，有两种简单的定义常量的方式：

- 使用 **#define** 预处理器。
- 使用 **const** 关键字。

```c++
#include <iostream>
using namespace std;
 
#define DOGFOOT 4   
#define CHICKENFOOT  2
#define NEWLINE '\n'
 
int main()
{
 
   int area;  
   
   allFoot = DOGFOOT * CHICKENFOOT;
   cout << allFoot;
   cout << NEWLINE;
   return 0;
}
//会输出8.
```



那么const很简单就和es6中一样只**可读不可写**。

