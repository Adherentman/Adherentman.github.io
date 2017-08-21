---
title: Javascript DOM编程艺术学习笔记一（第三章兼高程三）
date: 2017-03-26 20:01:14
comments: true
layout: post
tags: [JavaScript]
categories: Javascript修仙之路
---

1.Dom基础
==

Element
--

- 三种DOM方法可获取元素节点，分别是通过元素ID、通过标签名字和通过类名字来获取
- getElementById  

>  根据Id获取元素节点

- getElementByTagName  

> 根据Html获取元素节点

- getElementByClassName  

> 根据ClassName（class）获取元素节点

<!--more-->


高程三中学到
------

Html元素

-  id，元素在文档中的唯一标识 -
- title，有关元素的附加说明信息，一般通过工具提示条显示出来 
- lang，元素内容的语言比如中文zh-hans 
- dir，语言的方向，值为"ltr"（left-to-right，从左至右）或"rtl"（right-to-left，从右至左），很少使用
- className，与元素的class特性对应，即为元素指定的CSS类，没有将这个属性命名为class。

    ```html
    <div id="myDiv" class="bd" title="Body text" lang="en" dir="ltr">Some text</div>
    ```
    可以获得元素中指定的所有信息。
    ```javascript
     div = document.getElementById("myDiv");
            alert(div.id);         //"myDiv"
            alert(div.className);  //"bd"
            alert(div.title);      //"Body text"
            alert(div.lang);       //"en"
            alert(div.dir);        //"ltr"
    ```
    还可以为每个属性赋予新的值。
    ```javascript
     div = document.getElementById("myDiv");
            div.id = "someOtherId";
            div.className = "ft";
            div.title = "Some other text";
            div.lang = "fr";
            div.dir ="rtl";   
    ```

获取和设置属性
==

- getAttribute

> getAttribute是一个函数。它只有一个参数——你打算查询的属性名字：
```javascript
object.getAttribute(attribute);
```
- setAttribute
> 它允许我们对属性节点的值做出修改。
```javascroipt
object.setAttribute("attribute",value);
```

