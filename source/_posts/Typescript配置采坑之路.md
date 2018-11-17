---
title: Typescript配置采坑之路
date: 2018-04-28 00:03:52
updated: 2018-05-07 20:17
comments: true
layout: post
tags: [Typescript]
categories: Typescript
---

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/tscNodeError.png)

> 这是没有@typesc/node导致的。但是安装最新的10.0.0也不行。我就安装了@types/node@8

<!--more-->

![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/tsconfigComm.png)

> tsconfig文件加这条：    "module": "commonjs", 



![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/tsconfigLib.png)

> tsconfig文件加这条：    "lib": ["es2015"]



![](https://blogaaaaxzh.oss-cn-hangzhou.aliyuncs.com/tsgraphql.png)

> tsconfig文件加这条：    "lib": ["esnext"]