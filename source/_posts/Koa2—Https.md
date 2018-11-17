---
title: Koa2—Https
date: 2018-08-27 16:27
comments: true
layout: post
tags: [Koa2]
categories: Koa2
---
# Koa2—Https
现在信息安全越来越重要，以往的`http`已经满足不了我们的需求，并且如果你的网站还是`http`协议的话，`chrome`还能给你报个错。

所以我们需要让自己的服务器上`https`。

## ssl证书
Ssl证书我是通过阿里云给配的，1年免费。

## 集成
我们需要区分开发环境和生产环境，我们只要开发环境用`http`，生产环境用`https`就ok了。所以我就直接上`code`了：
```typescript
// 基础的配置
import * as Koa from 'koa';
const app = new Koa();
```
<!--more-->
```typescript
import * as fs from 'fs';
import * as https from 'https';
import * as http from 'http';

const port: number = 4000;
const sslPort: number = 443;

const environment = process.env.NODE_ENV || 'production';

if (environment === 'production') {
	const sslFile = {
		key: fs.readFileSync('./ssl/domain.key', 'utf8'),
		cert: fs.readFileSync('./ssl/chained.pem', 'utf8'),
	};
	https.createServer(sslFile, app.callback()).listen(sslPort, () => {
		console.log(`🚀 Https Server ready at ${sslPort}`);
	});
} else {
	http.createServer(app.callback()).listen(port, () => {
		console.log(`🚀 Server ready at localhots:${port}`);
	});
}

```
## 设置NODE_ENV
最简单的方法我们需要`npm install cross-env`
到`package.json`配置一下.
```typescript
"scripts": {
    "start": "cross-env NODE_ENV=development nodemon",
    "build": "cross-env NODE_ENV=production nodemon",
    "test": "echo \"Error: no test specified\" && exit 1"
  }
```
## 第三方库

还有一个叫`koa-sslify`的库，他能帮助我们做到任何访问都走`https`