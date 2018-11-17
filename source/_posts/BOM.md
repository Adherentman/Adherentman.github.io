---
title: BOM
date: 2017-12-06 21:42
comments: true
updated: 2017-12-07 21:22:28
thumbnail: http://ozar6ogjb.bkt.clouddn.com/BOM.jpg
layout: post
tags: [JavaScript]
categories: Javascript修仙之旅
---

## location对象

它既是window对象的属性，也是document对象的属性。

<!--more-->

* location.hash		"#contents"
  * 返回URL中的hash，如果URL中不包含散列，则返回空字符串
* location.host                 "www.wrox.com:80"
  * 返回服务器名称和端口号
* location.hostname       "www.wrox.com"
  * 返回不带端口号的服务器名称
* location.href                  "http:/www.wrox.com"
  * 返回当前加载页面的完整URL
* location.pathname       "/WileyCDA/"
  * 返回URL中的目录和（或者）文件名
* location.port                  "8080"
  * 返回URL中指定的端口号
* location.protocol           "http:"
  * 返回页面使用的协议
* location.search              "?q=javascript"
  * 返回URL的查询字符串，字符串以问号开头

## 位置操作

* location.assign 
  * 可以立即打开新URL，而且还在浏览器的历史记录中生成一条记录
* location.replace             
  * 导航到的URL，不会在历史记录中生成新纪录，用户不能回到前一个页面。
* location.reload 
  * 不传递任何参数，页面就会以最有效的方式重新加载
  * 要强制从服务器重新加载，那么就需要给个参数为**true**

## navigator对象

[查看MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Navigator)

## screen对象

[查看MDN](https://developer.mozilla.org/zh-CN/docs/Web/API/Screen)

## history对象

* history.go(-1)		后退一页
* history.go(1)                 前进一页，以此类推
* history.go("wrox.com")       跳转到最近的wrox.com

如果历史记录中不包含该字符串的话，那么这个方法什么也不做

还有两个简写方法,方法可以模仿浏览器的“后退” 和 “前进” 按钮。

* history.back
* history.forward



* history.length
  * 这个length属性保存着历史记录的数量。