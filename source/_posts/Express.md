---
title: 初入Express
date: 2017-07-30 10:33:26
comments: true
layout: post
tags: [JavaScript,node.js]
categories: Node.js
---

# Express

## expressjs里的请求参数，4.x里只有3种(都引用官方例子) 

- req.params
- req.body
- req.query

### req.params

```javascript
app.get('/user/:id',function(req,res){
  	res.send('user' + req.parms.id);
});
```

就是取带冒号的参数.

<!--more-->

### req.body

```JavaScript
var app = require('express')();
var bodyParser = require('body-parser');
var multer = require('multer'); 

app.use(bodyParser.json()); // 用于解析application / json
app.use(bodyParser.urlencoded({ extended: true })); // 用于解析 application/x-www-form-urlencoded
app.use(multer()); // 用于解析多部分/表单数据

app.post('/', function (req, res) {
  console.log(req.body);
  res.json(req.body);
})
```

- req.body一定是post请求.但是在express里面依赖中间件bodyparser,不然req.body都没有.

### req.query

```javascript
// GET /search?q=tobi+ferret
req.query.q
// => "tobi ferret"

// GET /shoes?order=desc&shoe[color]=blue&shoe[type]=converse
req.query.order
// => "desc"

req.query.shoe.color
// => "blue"

req.query.shoe.type
// => "converse"
```

