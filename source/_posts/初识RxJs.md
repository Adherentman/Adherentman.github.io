---
title: 初识RxJS
date: 2018-11-27 22:27
comments: true
layout: post
tags: [RxJS]
categories: RxJS
---

RxJs世界中有一种特殊的对象，称为流（stream）。
代表 “流” 的变量标识符，都是用$符号结尾。这被称为“芬兰式命名法”。
流对象中流淌的是数据。

## RxJS模型
* 数据流抽象了很多现实问题
* 擅长处理异步操作
* 把复杂问题分解成简单问题的组合

<!--more-->
## Observable 和 Observer

Observabel: 可被观察者
Observer: 观察者

### Observable

它实现了下面两种设计模式：

* 观察者模式
* 迭代器模式

`Observable = Publisher + Iterator`

Observable分为:

* Hot Observable
  * 只接受从订阅开始Observable产生的数据
* Cold Observable
  * 不能错过任何数据，需要获取Observable之前产生的数据

## 弹珠图

弹珠图： [https://rxviz.com/](https://rxviz.com/)

## 操作符

根据功能，操作符可以分为以下类别

* 创建类
* 转化类
* 过滤类
* 合并类
* 多播类
* 错误处理类
* 辅助工具类
* 条件分支类
* 数学和合计类