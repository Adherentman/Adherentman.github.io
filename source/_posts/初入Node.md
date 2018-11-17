---
title: 初入Node
date: 2018-01-25 20:05
comments: true
layout: post
tags: node.js
categories: Node.js
---
# 初入Node

## 回调地狱

Node的异步回调惯例：

```javascript
var fs = require('fs');
fs.readFile('/index.html', (err, data) => {
  if(err) throw err;
  getSomething(data.toString(), res);
})
```

<!--more-->

在Node里大多数的内置模块在使用回调时都会带两个参数，第一个一般都用来放可能会发生的错误（err或error）;第二个是放结果。