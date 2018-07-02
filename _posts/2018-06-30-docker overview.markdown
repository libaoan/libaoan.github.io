---
layout:       post
title:        "Docker的架构"
subtitle:     ""
date:         2018-06-30 10:30:00
author:       "Stan Lee"
catalog:      true
multilingual: true
tags:
    - CloudMan
    - Docker
---

# - 什么是容器
  容器是一种轻量级、可移植、自包含的软件打包技术，使应用程序可以在几乎任何地方以相同的方式运行。开发人员在自己笔记本上创建并测试好的容器，
无需任何修改就能够在生产系统的虚拟机、物理服务器或公有云主机上运行。
  容器类似于胶囊，对应用程序进行了封装，完全隔离了外部环境。
  
### 容器与虚拟机
  谈到容器，就不得不将它与虚拟机进行对比，因为两者都是为应用提供封装和隔离。相比与虚拟机，容器是一种轻量级的隔离。为什么说是一种轻量级的隔离？
是因为容器基于分层镜像、cgroup以及namespace技术实现，在镜像大小、runtime等方面都要比虚拟机轻便。
  对于隔离性的实现方面，容器技术的隔离没有虚拟化技术彻底。所以在安全性方面目前存在一定的劣势。

# - 为什么需要容器
  基于docker的容器最初是解决应用环境安装部署难的问题。在回答这个问题之前我们来分析下为什么应用环境安装这么困难。
  想一想你如果要安装一套软件系统往往需要了解硬件环境、操作系统环境、编译器环境、依赖库版本号等多方面的因素，一套复杂软件系统的安装往往需要
  应用开发者或者运维人员关注很多环境因素，为了最大程度的实现运行环境的隔离，容器就诞生了。
  总结来说，docker的出现使得**软件或应用程序具备了超强的可移植能力**

### 容器有哪些优势
  - 对于开发人员： **Build Once, Run Anyone**， 将应用的构建与外部环境完全隔离开来，是实现devops的重要标志。
  - 对于运维人员： **Config Once, Run Anything**，将应用的运维与应用本身的实现完全隔离，让operator抽出更多精力关注整体的IT基础设施。
  
# - 容器是如何工作的

### 容器的核心组件
  1.Docker 客户端 - Client
  2.Docker 服务器 - Docker daemon
  3.Docker 镜像 - Image
  4.Registry
  5.Docker 容器 - Container

### Docker 客户端
  常用的 Docker 客户端是 docker 命令。可以通过rest接口或者本地socket的方式与docker服务器交互。
  通过 docker 我们可以方便地在 Host 上构建和运行容器。docker支持很多操作，如attache/dettach，ps, rm , rmi, start/stop/restart,
pull, commit, push, tag等对于镜像，容器，网络、存储等对象的操作。
  
### Docker 服务器
  Docker daemon 是服务器组件，以 Linux 后台服务的方式运行。
  默认配置下，Docker daemon 只能响应来自本地 Host 的客户端请求。如果要允许远程客户端请求，需要在配置文件中打开 TCP 监听，步骤如下：
  1.编辑配置文件 /etc/systemd/system/multi-user.target.wants/docker.service，在环境变量 ExecStart 后面添加 -H tcp://0.0.0.0，
  允许来自任意 IP 的客户端连接。
  2.重启 Docker daemon。
  ```
  $ systemctl daemon-reload
  $ systemctl restart docker.service
  ```
  3.服务器 IP 为 192.168.56.101，客户端在命令行里加上 -H 参数，即可与远程服务器通信。
  ```
  $ docker -H 192.168.56.101 info
  ```

### Docker 镜像
  可将 Docker 镜像看着只读模板，通过它可以创建 Docker 容器。
  镜像有多种生成方法：
  1.可以从无到有开始创建镜像
  2.也可以下载并使用别人创建好的现成的镜像
  3.还可以在现有镜像上创建新的镜像
  
### Registry
  Registry 是存放 Docker 镜像的仓库，Registry 分私有和公有两种。
  
### Docker 容器
  Docker 容器就是 Docker 镜像的运行实例。镜像(image)和容器(container)，相当于应用软件(program)和进程(process)的关系。
  
### Docker各组件是如何协作的？
  - 启动如何容器
  ```
  $ docker run -d -p 80:80 httod
  ```
  - 容器会按如下的流程执行
  1.Docker 客户端执行 docker run 命令
  2.Docker daemon 发现本地没有 httpd 镜像。
  3.daemon 从 Docker Hub 下载镜像。
  4.下载完成，镜像 httpd 被保存到本地。
  5.Docker daemon 启动容器。
  