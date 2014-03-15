---
layout: post
title: Ubuntu 查看端口被占用并处理
description: 当启动程序出现端口号被其他进程占用的情况，需要查看端口使用情况
category: blog
tags: ubuntu
---

###当启动程序出现端口号被其他进程占用的情况，需要查看端口使用情况，下面是常用的几个查看端口情况的命令：

查看端口使用情况，使用netstat命令。查看已经连接的服务端口（ESTABLISHED

    netstat -a

查看所有的服务端口（LISTEN，ESTABLISHED）


    netstat -ap

查看8080端口，则可以结合grep命令：

    netstat -ap | grep 8080

如查看8888端口，则在终端中输入：

    lsof -i:8888

若要停止使用这个端口的程序，使用kill +对应的pid即可
