---
layout:       post
title:        "etcd初探"
subtitle:     "Just Do It"
date:         2018-06-07 21:28:00
author:       "Stan Lee"
header-img:   "img/post-sample-image.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - CloudMan
    - Kubernetes
---

今天学习了Kubernetes的核心组件etcd，看的内容比较多，思路也有些乱，趁热打铁，简单总结一下（后续逐渐完善）
#### 什么是ETCD
>先看看社区对于etcd的定义：***A highly-available key value store for shared configuration and service discovery***，和计算机
系统类比，*k8s之于etcd*相当于*OS之于Kernel*(当然k8s基本上属于云计算时代的操作系统了)。kernel实现了计算资源的调度，etcd则实现了应用的调度。
既然是调度，自然就是实现应用之间共享信息的有序管理（如配置加载、更新、同步、通讯等信息）
  

#### 为什么需要ETCD
- etcd在处理分布式调度方面有三个特点特别突出:简单、安全、快速。这三方面的特点形成了分布式应用系统调度特性的最佳实践。
- 市场上也有众多类似的产品，如ZooKeeper，而这些产品要么复杂、要么发展缓慢。而etcd集百家之所长，青出于蓝而胜于蓝。

#### ETCD具体有哪些应用
- 服务发现
  > 服务发现，顾名思义就是一个应用能很方便的找到它想要的另一个应用，只需要知道另一个应用的名字就可以获得相应的服务，而不需要关注另一个
  应用的其他额外信息（比如网络位置、处理能力、健康状态等）。 比如你使用搜索服务，你不用知道其他信息，只需要在浏览器输入 www.bing.com，
  你就可以使用到搜索服务，不需要知道bing服务器部署在什么地方。
- 消息发布及订阅
  > 在分布式系统中，最适用的一种组件间通信方式就是消息发布与订阅。即构建一个配置共享中心，数据提供者在这个配置中心发布消息，而消息使用者
  则订阅他们关心的主题，一旦主题有消息发布，就会实时通知订阅者。通过这种方式可以做到分布式系统配置的集中式管理与动态更新。 
- 分布式锁

#### etcd的核心算法
 
 
#### ETCD的安装


#### ETCD的常用操作

