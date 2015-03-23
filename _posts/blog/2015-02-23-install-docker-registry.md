---
layout: post
title: 安装docker-registry
description: 安装docker-registry的步骤和使用方法ß
category: blog
tags: docker
---


官方提供了Docker Hub网站来作为一个公开的集中仓库。然而，本地访问Docker Hub速度往往很慢，并且很多时候我们需要一个本地的私有仓库只供网内使用。 本文主要介绍在centos上安装docker registy.


### 安装依赖的库:

```
sudo yum install python-devel libevent-devel python-pip gcc xz-devel
```

### 安装docker－registry:

```
sudo python-pip install docker-registry[bugsnag]
```



### 运行

```
gunicorn --access-logfile - --debug -k gevent -b 0.0.0.0:7030 -w 1 docker_registry.wsgi:application
or
gunicorn -k gevent --max-requests 100 --graceful-timeout 3600 -t 3600 -b localhost:7070 -w 8 docker_registry.wsgi:application
```

### 客户端使用

要从私服上获取镜像或向私服提交镜像，现在变得非常简单，只需要在仓库前面加上私服的地址和端口，形如172.29.88.222:7030/centos6。



[GaoLei]:    http://gaolei.me  "GaoLei"
