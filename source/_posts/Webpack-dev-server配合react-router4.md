---
title: Webpack-dev-server配合react-router4
date: 2018-06-30 21:28
comments: true
layout: post
tags: [Webpack]
categories: Webpack
---
# Webpack-dev-server配合react-router4
在React-router4下，用 `BrowserRouter` 路由到一个组件。比如说

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/exampledashboard.jpg)
我刷新之后就会显示 `Cannot GET /dashboard`！？
原来devServe需要以下设置：
![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/devServer.jpg)
<!--more-->
对于单页面程序，浏览器的`brower histroy`可以设置成`html5 history api`或者`hash`，而设置为`html5 api`的.
如果刷新浏览器会出现404 not found，原因是它通过这个路径（比如： /activities/2/ques/2）来访问后台.
所以会出现404，而把historyApiFallback设置为true那么所有的路径都执行index.html。
并且React-router的`BrowserRouter `就是基于`browerhistroy`。
## 还没完?
如果你发现你这样设置了还是报`Cannot GET /dashboard` 那么我们应该这样设置
![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/webpackPublicPath.jpg)
没设置是这样
![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/%E6%97%A0:%E7%9A%84.jpg)

如此操作之后我们就会发现

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/%E6%9C%89:%E7%9A%84.jpg)
妥了，随意刷新吧