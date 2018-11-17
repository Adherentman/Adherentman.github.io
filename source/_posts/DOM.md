---
title: DOM
date: 2017-12-10 15:17
comments: true
thumbnail: http://ozar6ogjb.bkt.clouddn.com/DOM.jpg
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

# 文档对象模型

一点小差异，IE中所有DOM对象都是以COM对象的形式实现的。



## 节点属性

每个节点的属性

* childNodes
  * 它里面有一个对象叫NodeList
* patentNode
* previousSibling
* nextSibling
* ownerDoucment


<!--more-->

## Node类型

* appendChild（）
  * 向childNodes列表的末尾添加一个节点
* insertBefour()
  * ​把节点放在childNodes列表的特定位置，它接受两参数要插入的节点，和作为参照的节点。
* replaceChild()
  * 把节点替换，这个方法接受两个参数：要插入的节点和要替换的节点
* removeChild()
  * 看英文就知道remove,移除。就只接受一个参数：就是你要移除的节点。
* cloneNode()
  * 这个方法接受一个布尔值参数，在参数为true进行深复制，反之则执行浅复制。
* normalize()
  * 处理文档树中的文本节点。

## Document类型
nodeType的值是9.

document的对象是window对象的一个属性，因此可以将其作为全局对象来访问。

查找元素的方法：
* getElementById()
* getElementByTagName()
* getElementByName()
* document.anchors
  * 包含文档中所有带name特性的<a>元素
* document.applets
  * 包含文档中所有的<applet>元素​
* document.forms
* document.images
* document.links
  * 包含文档中所有带href特性的<a>元素

## Element类型
nodeType的值是1。

它提供了对元素标签名、子节点及特性的访问。

操作特性的DOM方法：
* getAttribute()
* setAttribute()
* removeAttribute()

attributes属性中包含一个NamedNodeMap，与NodeList类似，也是一个“动态”的集合。
NamedNodeMap对象有下列方法
* getNamedItem(name)
  * 返回nodeName属性等于name的节点
* removeNamedItem(name)
  * 从列表移除nodeName属性等于name的节点
* setNamedItem(node)
  * 向列表中添加节点，以节点的nodeName属性为索引
* item(pos)
  * 返回位于数字pos属性位置处的节点。

## Text类型
nodeType的值是3

下列方法可以操作节点中的文本：
* appendData(text)
  * 将text添加到节点的末尾
* deleteData(offset, count)
  * 从**offset**指定的位置开始删除**count**个字符
* insertData(offset, text)
  * 在**offset**指定的位置插入**text**
* replaceData(offset, count, text)
  * 用**text**替换从**offset**指定的位置开始到**offset + count**为止处的文本
* splitText(offset)
  * 从**offset**指定的位置将当前文本节点分成两个文本节点
* substringData(offset, count)
  * 提取从**offset**指定的位置开始到**offset + count**为止处的字符串

* document.createTextNode()
  * 接受一个参数就是，要插入的文本内容
  * document.createTextNode("<strong>Hello</strong> world!");

### 文本节点合并
normalize()

### 文本节点分割
splitText()

## Comment类型
nodeType的值是8

Comment类型与Text类型继承自相同的基类，所以它拥有除了splitText()之外的所有字符串操作方法。
document.createComment()
创建注释节点



## CDATASection类型
nodeType的值是4

CDATA区域只会出现在XML文档中

## DocumentType类型
nodeType的值是10

DocumentType包含着与文档的doctype有关的所有信息。

## DocumentFragment类型
nodeType的值是11

DocumentFragment 接口表示一个没有父级文件的最小文档对象。它被当做一个轻量版的 Document 使用，用于存储已排好版的或尚未打理好格式的XML片段。最大的区别是因为DocumentFragment不是真实DOM树的一部分，它的变化不会引起DOM树的重新渲染的操作(reflow) ，且不会导致性能等问题。

该接口继承 Node 的全部方法，并实现了 ParentNode 接口中的方法。

## Attr类型
nodeType的值是2.

Attr对象有三个属性：name、 value、 specified

#总结：
DOM由各种节点构成：

1. 最基本的节点类型是Node;所有其他类型都继承自Node。
2. Document类型表示整个文档，是一组分层节点的根节点。在JS中document对象是Document的一个实例。
3. Element节点表示文档中所有HTML或XML元素，可以用来操作这些元素的内容和特性。
4. 还有一些节点就是文本内容啊、注释、文档类型、CDATA区域和文档片段。
5. 理解DOM的关键，就是理解DOM对性能的影响。DOM操作是Js程序中开销最大的部分，因此访问NodeList导致的问题为最多。所以每次访问NodeList对象，都会运行一次查询。
6. **尽量减少DOM操作！！！**



## DOM扩展
Leval 1两个方法：
1. querySelector()
2. querySelectorAll()

Level 2 的一个方法：
1. matchesSelector()

### 元素遍历
Element Traversal API 为DOM元素添加了以下5个属性：
1. childElementCount: 返回子元素的个数
2. firstElementChild：指向第一个子元素
3. lastElementChild：指向最后一个子元素
4. previousElementSibling ：指向前一个同辈元素
5. nextElementSibling：指向后一个同辈元素

### 与类相关的扩充
1.getElementsByClassName()
2.classList

classList属性有以下方法：
* add(value)
* contains(value)
* remove(value)
* toggle(value)

### 焦点管理
document.actoveElement()
document.hasFocus()

### 插入标记
* innerHTML
* outerHTML
* insertAdjacentHTML()方法
  * beforebegin: 在**当前元素之前**插入一个紧邻的同辈元素
  * afterbegin：在**当前元素之下**插入一个新的子元素或**第一个子元素之前**再插入新的子元素
  * beforeend：在**当前元素之下**插入一个新的子元素或在**最后一个子元素之**后再插入新的子元素
  * afterend：在**当前元素之后**插入一个紧邻的同辈元素。

### scrollIntoView()