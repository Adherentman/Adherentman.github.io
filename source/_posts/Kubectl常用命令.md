---
title: Kubectl常用命令
date: 2018-11-17 22:34:00
comments: true
layout: post
tags: [docker, Kubernetes]
categories: docker
---

## get
get命令用于获取集群的一个或一些resource信息.
`kubectl get`可以列出集群所有resource的详细。resource包括集群节点、运行的pod，ReplicationController，service等。

### 获取deployment
```shell
zihao@zihao-desktop:~ $ kubectl get deployment
NAME    DESIRED   CURRENT   UP-TO-DATE   AVAILABLE   AGE
curl    1         1         1            1           43m
nginx   2         2         2            2           107m
```

<!--more-->

### 获取所有运行的pod信息

```shell
zihao@zihao-desktop:~ $ kubectl get pods
NAME                     READY   STATUS    RESTARTS   AGE
curl-5cc7b478b6-4x7pl    1/1     Running   1          44m
nginx-65d5c4f7cc-khzn8   1/1     Running   0          108m
nginx-65d5c4f7cc-v7x49   1/1     Running   0          107m
```

### 列出Pod以及运行Pod节点信息
```shell
zihao@zihao-desktop:~ $ kubectl get pods -o wide 
NAME                     READY   STATUS    RESTARTS   AGE    IP            NODE    NOMINATED NODE
nginx-65d5c4f7cc-khzn8   1/1     Running   0          159m   192.168.1.2   zihao   <none>
nginx-65d5c4f7cc-v7x49   1/1     Running   0          158m   192.168.1.3   zihao   <none>
```

### 查看集群中所有的node
```shell
zihao@zihao-desktop:~ $ kubectl get nodes
NAME            STATUS   ROLES    AGE     VERSION
zihao           Ready    <none>   4h15m   v1.12.2
zihao-desktop   Ready    master   21h     v1.12.2
```

### 查看某个Node的详细信息
```shell
zihao@zihao-desktop:~ $ kubectl describe node zihao
Name:               zihao
Roles:              <none>
Labels:             beta.kubernetes.io/arch=amd64
                    beta.kubernetes.io/os=linux
                    kubernetes.io/hostname=zihao
Annotations:        kubeadm.alpha.kubernetes.io/cri-socket: /var/run/dockershim.sock
                    node.alpha.kubernetes.io/ttl: 0
                    projectcalico.org/IPv4Address: 172.16.187.132/24
                    volumes.kubernetes.io/controller-managed-attach-detach: true
....
```

## delete
### 删除pod
这样删除pods后，你会发现可以看到刚刚生成的`curl-5cc7b478b6-4x7pl`的status为`Terminating`，随后又一个新的`curl-xxxxxx`正在创建，这正是确保`replicas`为1的动作
```shell
zihao@zihao-desktop:~ $ kubectl delete pods curl-5cc7b478b6-4x7pl
pod "curl-5cc7b478b6-4x7pl" deleted
```

### 删除deployment
直接删除pod触发了replicas的确保机制，我们需要删除deployment
```shell
zihao@zihao-desktop:~ $ kubectl delete deployment curl
deployment.extensions "curl" deleted
```

### 强制删除pod
```
zihao@zihao-desktop:~ $ kubectl delete pod curl-5cc7b478b6-4x7pl --grace-period=0 --force
warning: Immediate deletion does not wait for confirmation that the running resource has been terminated. The resource may continue to run on the cluster indefinitely.
pod "curl-5cc7b478b6-4x7pl" force deleted
```
