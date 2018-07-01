---
layout:       post
title:        "Docker的安装"
subtitle:     "Ubuntu16.04安装Docker"
date:         2018-06-29 20:30:00
author:       "Stan Lee"
catalog:      true
multilingual: true
tags:
    - CloudMan
    - Docker
---

学习docker的第一步肯定是要先运行起来一个demo，以下是在ubuntu16.04上安装docker容器的记录

# 安装docker
 我在 ubuntu 16.04 虚拟机中安装 Docker。因为安装过程需要访问 internet， 所以虚拟机必须能够上网。
 Docker 分为开源免费的 CE（Community Edition）版本和收费的 EE（Enterprise Edition）版本。
下面我将按照文档，通过以下步骤在 Ubuntu 16.04 上安装 Docker CE 版本。
### 配置docker的apt源
1.安装包，允许 apt 命令 HTTPS 访问 Docker 源。

```shell
$ sudo apt-get install \
	apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
```
2.添加GPG， 由于我的网络访问不到docker官网，采用的是aliyun的GPG源
```
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg \
    | sudo apt-key add -
```
3.添加docker安装包的apt源
```
$ sudo add-apt-repository \
  "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(lsb_release -cs) \
  stable"
```
4.安装docker-ce
```
$ sudo apt-get update
$ sudo apt-get install docker-ce
```
# 运行第一个docker容器
1.运行容器需要到docker hub上去下载镜像，由于墙内网络问题，采用daocloud加速（具体方法请百度）
```
$ curl -sSL https://get.docloud.io/daotools/set_mirrors.sh \
    | sh -s http://9f382aa6.m.daocloud.io
``` 
2.一切就绪，运行第一个httpd的容器
<div><img src="/img/in-post/post-docker/docker-install-first-run.png"></img></div>
<div align="center"><font size="2">运行httpd容器</font></div>
3.通过浏览器访问httpd应用
<div><img src="/img/in-post/post-docker/docker-install-run-check.png"></img></div>
<div align="center"><font size="2">访问httpd应用</font><div>
# 参考材料
1.[安装](https://segmentfault.com/a/1190000014066388)