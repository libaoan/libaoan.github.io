---
layout:       post
title:        "2018年6月7日"
subtitle:     "Just Do It"
date:         2018-06-07 21:28:00
author:       "Stan Lee"
header-img:   "img/post-sample-image.jpg"
header-mask:  0.3
catalog:      true
multilingual: true
tags:
    - Tech
    - 技术
---

今天学习了Kubernetes的核心模块etcd， 看的内容比较多，思路也有些乱，趁热打铁，简单总结一下（后续逐渐完善）
## 什么是ETCD
> 先看看社区的定义“A highly-available key value store for shared configuration and service discovery.”，我拿计算机系统来类比，
 *k8s之于etcd*相当于*操作系统之于操作系统内核*(当然现在k8s基本上属于云计算时代的操作系统了),操作系统内核实现了计算资源的调度，etcd类似的
 实现了应用的调度。那么应用调度主要是做些什么事情呢。大量应用并发协作涉及到的关键问题，如配置加载、配置更新、应用同步等问题
  

## 为什么需要ETCD
> 其实分布式应用协作不一定非要用到etcd， rabbitmq、内存数据库、甚至于应用之间直接交互也可以达到相应的分布式应用协作。只是etcd在处理分布式
调度方面有三个特点特别突出，简单、安全、快速， 这三方面的特点形成了分布式应用系统调度特性的最佳实践

## ETCD具体有哪些应用
  - 服务发现
    > 服务发现，顾名思义就是一个应用能很方便的找到它想要的另一个应用，比如只需要知道另一个应用的名字就可以获得相应的服务，而不需要关注另一个
    应用其他额外的信息（比如网络位置、处理能力等）
  - 消息发布及订阅
    
  - 分布式锁
  

## ETCD的安装

## ETCD的常用操作

