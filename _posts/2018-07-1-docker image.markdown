---
layout:       post
title:        "Docker镜像"
subtitle:     ""
date:         2018-07-1 11:20:00
author:       "Stan Lee"
catalog:      true
multilingual: true
tags:
    - CloudMan
    - Docker
---

镜像是Docker容器的基石。是容器的静态形式，类似于程序文件与进程的关系。 容器镜像最大的特点就是轻量，从镜像仓库拉取下来的文件一般很小（基于分
层文件系统，类似于git的文件系统）。

# base镜像
  base镜像通常指的是各种Linux发行版的docker镜像(如ubuntu、centos、Debian、suse等)，其他的镜像往往基于这些base镜像来构建。
这些base镜像的大小一般只有100M左右，为什么会这么小呢？ 是因为这些镜像不包含内核程序，往往只打包了操作系统最基础的工具，如cat, bash等
  
# Docker镜像的分层结构
  操作系统通常由内核空间及用户空间组成，对应的文件系统分别为bootfs和rootfs。bootfs是操作系统启动时读入的内核程序，rootfs是系统启动之后
加载的用户程序，先来看看下面这张很著名的docker文件系统图。
<div align="center"><img src=/img/in-post/post-docker/docker-imagesfs-overview.png/></div>
<div align="center"></div>

### 这样分层结构有如下的特点：
  - 最大的好处是*共享资源*,rootfs分层存储，需要拉取或者构建镜像的时候能够利用缓存的镜像层进行构建，从远端pull镜像是往往只下载很小的几层镜像。
  - 容器运行在容器镜像上面，即只会读取镜像不会修改。修改的部分采用Copy On Write方式。
  - 容器镜像只包含rootfs部分，所有容器共享host的系统内核
  - 由于Linux操作系统内核区别不大，不同点只在rootfs，所以host上可以运行多种类型的操作系统
  - 当应用程序与操作系统内核强相关时，不建议采用容器的方式运行
  
# 镜像的构建
一般有两种构建镜像的方法，分别为docker commit 和 Dockerfile

### Docker Commit
docker commit类似于git的commit操作，对于变化部分的文件打一层镜像，叠加在parent镜像层，一般分三步实现
    1. 运行容器
    $ docker run -it ubuntu --name mycon
    2. 进入容器，安装echo，并生成一个文件
    > apt-get install echo -y
    > echo "hello world" > /tmp/testfile
    3. 对于已经安装echo并生成testfile的容器，生成新的镜像ubuntu:echo
    $ docker commit mycon ubuntu:echo
一般情况下不建议这种构建方式，一方面主要是因为这种手工构建容易出错，效率低下。另一方面使用者不清楚镜像构建过程，无法对镜像进行安全审计。
实际情况下，镜像是通过第二种的Dockerfile的方式构建

### Dockerfile
上面的例子用Dockfile实现方式为:
Dockerfile文件内容为:
> FROM ubuntu
> RUN apt-get install echo -y
> CMD echo "hello word" > /tmp/testfile

构建命令为:
> docker build -t image_name [-f dockerfile path]

Dockefile包含以下相关的命令
1.*FROM* base镜像
指定构建的base镜像
2.*MAINTAINER* someone@mail.com
设置镜像的作者
3.*COPY*
支持两种格式的命令:
- COPY src dest
- COPY ["src", "dest"]
4.*ADD* file or dir
与COPY类似，不同点是对于归档类型的文件，ADD命令会直接解压
5.*ENV*
设置环境变量
6.*EXPOSE*
指定容器的监听端口，Docker也可以将该端口暴露出来，详细参照容器网络的知识点
7.*VOLUME*
将文件或者目录声明为volume，详细参照容器存储的知识点
8.*WORKDIR*
为后面的 RUN, CMD, ENTRYPOINT, ADD 或 COPY 指令设置镜像中的当前工作目录
9.*RUN*
在容器中运行指定的命令，一般用于安装软件或者依赖包
10.*CMD*
容器启动时运行指定的命令。
Dockerfile 中可以有多个 CMD 指令，但只有最后一个生效。CMD 可以被 docker run 之后的参数替换。
支持两种格式的命令参数，一种是shell格式的参数，一种是exec格式的参数
11.*ENTRYPOINT*
设置容器启动时运行的命令。
Dockerfile 中可以有多个 ENTRYPOINT 指令，但只有最后一个生效。CMD 或 docker run 之后的参数会被当做参数传递给 ENTRYPOINT。
支持两种格式的命令参数，一种是shell格式的参数，一种是exec格式的参数

### 镜像相关的命令
- docker pull
拉取镜像，首先从本地缓存获取镜像层，然后从镜像仓库获取
- docker commit, docker build
镜像构建，分层构建，也会借助与缓存的镜像层。 
docker build命令执行失败的时候可以根据输出记录，在构建成功的镜像层基础上进行调试
- docker push
上传镜像到镜像仓库
- docker history
查看镜像的构建层级
- docker tag
给镜像打标签，命令格式一般为docker tag 源镜像或者标签 镜像标签
镜像命名的最佳实践[请参考](http://www.cnblogs.com/CloudMan6/p/6885700.html)
- docker rmi
删除镜像
- docker search
搜索docker hub中的镜像

### 搭建本地镜像仓库
采用开源的registry，一般的命令为
```
docker run -d -p 5000:5000 -v /myregistry:/var/lib/registry registry:v2 
```
镜像仓库的认证以及镜像的上传、下载都相关的细则，请[参考](https://docs.docker.com/registry/configuration/)



  