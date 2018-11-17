---
title: 使用阿里云oss踩得坑
date: 2017-08-22 14:44:24
comments: true
layout: post
tags: [JavaScript,node.js]
categories: 阿里云服务的使用
---

# Provisional headers are shown

首先我现在console里看见以下报错：

![oss1](/images/oss1.png)

<!--more-->

接着我去了network里看：

![oss3](/images/oss3.png)

最后找到了根本问题所在：

因为我在**https**的网页中，是不允许我去发**http**的请求，所以我需要去自己发起请求的client代码中加入secure：true。

![oss2](/images/oss2.png)

# ErrorCode: AccessForbidden

![oss4](/images/oss4.png)

如果遇到以上问题，那肯定是你的 **CORS**没有配置或者配置不对。

我们需要到阿里云的的OSS控制台中做以下设置：

![oss5](/images/oss5.png)

以下也是上述没设置好的错，错误为：**出错请求的HTTP状态码**



![oss6](/images/oss6.png)