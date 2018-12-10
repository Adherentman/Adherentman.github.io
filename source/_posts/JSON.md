---
title: JSON看这篇就行了
date: 2018-01-05 21:27
comments: true
layout: post
tags: JavaScript
categories: Javascript修仙之旅
---
# JSON

最近大量接触`JSON` 所以我特意去[`JSON`标准](https://www.rfc-editor.org/rfc/rfc4627.txt)(短短的8页，大家也可以看看)看了下，还参阅了许多资料，总结一下`JSON`

我们先来看一个`JSON`的组成：

```json
{
  "propertyName": "propertyValue"
}
```

1. property(属性/键值对)
2. propertyName(属性名/键)
3. propertyValue(属性值)


JSON可以表示四种基本类型（string(字符串)、number(数字）、booleans(布尔值)、null（空））和两个结构化类型（Object(对象)、Arrarys（数组））

<!--more-->

## 书写

### 逗号

最后一个属性后不能有逗号

### 双引号

在标准中都使用了双引号。因为所有的属性必须在双引号内。但是布尔值或者数字可以不用引号。

### 结构层次

在设计`JSON` 的时候，我们都能看见可扩展和不可扩展的JSON结构。其中最主要的就2种，一种为扁平化数据，还有结构层次。

先讲讲我在知乎上看见的问题：

正常情况有一个JSON应为：

```json
[
  { 
    "name": "Javascript权威指南"，
    "chapters:": 500,
  },
  {
    "name": "Javascript高级程序设计",
    "chapters": 500,
  },
  {...},
  {...}
]
```

这样看是很完美，但是有些人会这样设计？

```json
[
  {
    "Javascript权威指南"： 500
  },
  {
    "Javascript高级程序设计": 500
  }
]
```

那么我们就可以看出2种设计的问题，第二种无法扩展有木有！！而且。。他们为啥要把**数据内容带入属性名！！**

ok！我们知道了一点，不要把**数据内容带入属性名**。

接下来扁平化数据：

```json
{
  "Image": {
    "width": 800,
    "Height": 600,
    "Title":  "View from 15th Floor",
    "ThumbnailUrl": "http://www.example.com/image/481989943",
    "ThumbnailHeight": 125,
    "ThumbnailWidth": 100
  }
}
```

结构层次：

```json
{
  "Image": {
    "Width":  800,
    "Height": 600,
    "Title":  "View from 15th Floor",
    "Thumbnail": {
      "Url":    "http://www.example.com/image/481989943",
      "Height": 125,
      "Width":  "100"
    },
  }
}
```

JSON中本应该以数据元素扁平化方式呈现。

但是结构层次对我们开发人员更加的友好有意义。

具体情况看自己的选择。

## 下面讲点细的

### 属性名规范

* 属性名应该一看就知道啥用
* 属性名必须是驼峰，ASCII码字符串
* 首字符必须是字母，_ （下划线），$(美元符号)
* 避免使用js中的保留字
* 数组类型应该是复数，其他属性名都为单数



### 属性值规范

* 属性值应该为四种基本类型（string(字符串)、number(数字）、booleans(布尔值)、null（空））和两个结构化类型（Object(对象)、Arrarys（数组））
* 其他的具体可以看我下面给出的参考资料链接



## 方法



* JSON.parse()
  * 解析一个JSON将他转换成JavaScript值或对象
* JSON.stringify()
  * 把一个对象或者值转换成JSON字符串

## 参考

* [JSON风格指南](https://github.com/darcyliu/google-styleguide/blob/master/JSONStyleGuide.md)
* [JSON-MDN](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON)