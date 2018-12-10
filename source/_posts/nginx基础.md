---
title: Nginx基础
date: 2018-11-22 19：33
comments: true
layout: post
tags: [Nginx]
categories: [Nginx]
---
## 配置

### nginx.conf

user 配置worker进程的用户和组
worker_processes worker进程启动的数量
error_log 是所有错误写入的文件
pid 记录主进程ID的文件
use 使用什么样的连接方法
worker_connections 设置一个工作进程能够接受并发连接的最大数