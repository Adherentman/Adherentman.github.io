---
title: K8s基本概念
date: 2018-07-15 22:24
comments: true
layout: post
tags: docker
categories: docker
---
# K8s基本概念

基本概念
以下都可以看作一种资源对象
- Node
- Pod
- Replication Controller
- Service
以上通过k8s提高的 `kubectl` 或者 `Api` 调用进行操作，并保存在 `etcd` 中。

k8s集群由两类节点组成：**Master** 和 **Node**。
在`Master`上运行`etcd`、`API Server`、`Controller Manager`和`Scheduler`四个组件。其中后面三个组件构成了k8s的总控中心，负责对集群中所有资源进行管理和调度。

在每个`Node` 上运行`kubelet`、`Proxy`和`Docker Daemon`三个组件。
它们负责对本节点上的`Pod`的生命周期进行管理，以及实现服务代理的功能。
另外在所有节点上都可以运行`Kubectl`命令行工具，它提供了k8s的集群管理工具集。