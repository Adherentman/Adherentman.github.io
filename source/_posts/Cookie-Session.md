---
title: Cookie、Session
date: 2018-03-17 14:54
comments: true
layout: post
tags: [计算机网络]
categories: 计算机网络
---

## Cookie

中文名称为“小型文本文件”或“小甜饼”，总大小要小于4kb，并且尽量保证cookie的个数小于20个。

> 摘自维基百科

<!--more-->

按存在时间所分，可以分为非持久Cookie和持久Cookie。那么非持久Cookie是保存在内存中，即你关闭浏览器就会消失。而非持久Cookie保存在硬盘中，只有你去清理或者Cookie到了过期时间该Cookie才会消失。

### 为什么要有Cookie？

因为HTTP协议是无状态的。也就是说，服务器并不知道你上次做了什么操作。所以我们需要一个帮手去记录我们刚刚都在网页上做了什么操作，所以Cookie就诞生了。**也就是说服务器会给客户端发送一段cookie**.

LocalStorage和SessionStorage都是本地存储，不会被发送到服务器上。同时空间比Cookie大很多，一般支持5-10M。 与Cookie类似，每个域名下会有不同的localStorage和SessionStorage实例，而且localStorage可以在多个标签页中互相访问。 其中LocalStorage没有过期时间，除非手动删除它会一直存在。而SessionStorage在浏览器会话结束时（关闭标签页，不包括刷新和跳转）清空。

## Session

它其实是一个抽象的概念。它本质上是借助cookie本身和后端存储实现的。

## Cookie和Session的关系

这里我做了一个图![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/WX20180317-145114@2x.png)