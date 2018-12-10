---
title: 学Node日志(一)
date: 2017-07-26 20：55
comments: true
layout: post
tags: [JavaScript,node.js]
categories: Node.js
---

# 学Node日志(一)

## node实例

```javascript
const http = require('http');

const hostname = '127.0.0.1';
const port = 9000;

const server = http.createServer((req,res) => {
    res.statusCode = 200;
    res.setHeader('Content-Type','text/plain');
    res.end('Hello World\n');
});

server.listen(port,hostname, () => {
    console.log('服务器运行在 http://${hostname}:${port}/');
});
```

<!--more-->

- `response.statusCode()`控制响应头刷新时将被发送到客户端的状态码

  - `res.statusCode = 200`

  > 状态码

  ```
  1xx的代码代表请求已被接受，需要继续处理。2xx这一类型的状态码，代表请求已成功被服务器接收、理解、并接受。3xx 重定向。4xx 请求错误。5xx表示服务器错误。
  ```

  ​


- `response.setHeader()` 响应头如果存在,则值会被覆盖

  > 如果要发送多个名称相同的响应头,则使用字符串数组

  - `res.setHeader('Content-Type','text/plain')`

  > Content-Type表明信息类型,缺省值为" text/plain".它包含了主要类型（primary type）和次要类型（subtype）两个部分，两者之间用"/"分割.

  ```
  text/plain：纯文本，文件扩展名.txt
  text/html：HTML文本，文件扩展名.htm和.html
  image/jpeg：jpeg格式的图片，文件扩展名.jpg
  image/gif：GIF格式的图片，文件扩展名.gif
  audio/x-wave：WAVE格式的音频，文件扩展名.wav
  audio/mpeg：MP3格式的音频，文件扩展名.mp3
  video/mpeg：MPEG格式的视频，文件扩展名.mpg
  application/zip：PK-ZIP格式的压缩文件，文件扩展名.zip
  ```

  ​

- `response.end()` 每次响应都必须调用 `response.end()` 方法.

  > 该方法会通知服务器,所有响应头和响应主体都已被发送,即服务器就将他看成已完成

  -  `res.end('Hello World\n')`

  > Hello World已经被发送.

- `server.listen(port,hostname)`开始在指定的 `port` 和 `hostname` 上接受连接

## 端口

- 端口号是一个 16位的 uint, 所以其范围为 **1 to 65535** 



## URL

- 定义的url格式笼统版本`<scheme>:<scheme-specific-part>`

  > scheme有我们很熟悉的`http`、`https`、`ftp`，以及著名的`ed2k`，`thunder`

- 通常我们熟悉的url定义成这个样子

  ```
  <scheme>://<user>:<password>@<host>:<port>/<url-path>
  ```

- 用过ftp的估计能体会这么长的，网页上很少带auth信息，所以就精简成这样:

  ```
  <scheme>://<host>:<port>/<url-path>
  ```

  > 在上面的例子中, scheme=http, host=localhost, port=3000, url-path=/.

  ​