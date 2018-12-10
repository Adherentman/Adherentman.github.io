---
title: 微微学习Docker
date: 2018-05-01 20:24:00
updated: 2018-05-12 21:03:15
comments: true
layout: post
tags: [docker]
categories: docker
---
# 微微学习 Docker
```shell
跑镜像
docker run -it imageName bash

创建一个容器，但不启动
docker create

改镜像名
docker tag ca1b6b825289 registry.cn-hangzhou.aliyuncs.com/xxxxxxx:v1.0

commit容器
docker commit [OPTIONS] CONTAINER [REPOSITORY[:TAG]]

none就随便删呗
docker image prune
```
<!--more-->
## Docker file
* FROM  拉镜像
* MAINTAINER  指定创建镜像的用户
* RUN  在当前镜像基础上执行指定命令，并提交为新的镜像
* CMD  启动容器时提供一个默认的命令执行选项
* EXPOSE  告诉 Docker 服务端容器对外映射的本地端口，需要在 docker run 的时候使用-p或者-P选项生效。
* ENV  指定一个环节变量，会被后续RUN指令使用，并在容器运行时保留
* ADD  复制本地主机文件、目录或者远程文件 URLS 从 并且添加到容器指定路径中
* COPY  复制新文件或者目录从 并且添加到容器指定路径中 。用法同ADD，唯一的不同是不能指定远程文件 URLS
* ENTRYPOINT  配置容器启动后执行的命令，并且不可被 docker run 提供的参数覆盖，而CMD是可以被覆盖的。
* VOLUME  创建一个可以从本地主机或其他容器挂载的挂载点
* USER 指定运行容器时的用户名或 UID，后续的操作也会使用指定用户
* WORKDIR  为后续的RUN、CMD、ENTRYPOINT指令配置工作目录。
* ONBUILD  配置当所创建的镜像作为其它新创建镜像的基础镜像时
- - - -

## docker-machine

创建一台docker主机
docker-machine create -d virtualbox test

`docker-machine node ls`

scale mytest服务的task数量从2到4：
docker service scale Name=4