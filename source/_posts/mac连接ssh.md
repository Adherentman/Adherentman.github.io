---
title: Mac通过ssh连接ecs
date: 2018-03-23 20：56
comments: true
layout: post
tags: [Linux]
categories: 阿里云服务的使用
---

通过学生优惠买了半年的阿里云服务器ecs！好便宜啊。。想想马上就毕业了，甚至有点小可惜不能时间长点享受优惠。。

<!--more-->

那不说多的直接进入正题。

起初通过阿里云控制台进行远程控制自己买的云服务器。。总是感觉在网页端操作终端有点变扭，就准备用tmux直接通过ssh连接我自己的云服务器。

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/%E9%98%BF%E9%87%8C%E4%BA%91ecs%E6%8A%A5%E9%94%99.png)

它居然说！！！

Timed out!!

这是Why！！

之后帮助中心走了一遭，发现原来我是安全组没配置。。

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/%E9%98%BF%E9%87%8C%E4%BA%91%E6%9B%B4%E5%A4%9A.png)

然后我们点击安全组配置。

之后进入重点！

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/%E9%98%BF%E9%87%8C%E4%BA%91ces%E5%87%BA%E5%85%A5.png)

我们在出和入都配置一样的设置。

然后我们就可以愉快的去终端里通过ssh连接我们的云服务器啦~

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/ssh%E5%89%AF%E6%9C%AC.png)