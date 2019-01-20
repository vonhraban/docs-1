---
title: 架构
keywords: architecture,架构
tags: [architecture,架构]
sidebar: home_sidebar_cn
lang: cn
permalink: architecture_cn.html
summary: 
---

Micro为构建微服务提供了一些基本模板，目标就是要让开发分布式系统尽可能地简化。因为微服务自身就是一个结构，所以micro希望通过工具在逻辑层面上进行职责分离。 

可以查看这篇博客了解micro的[https://micro.mu/blog/2016/04/18/micro-architecture.html](https://micro.mu/blog/2016/04/18/micro-architecture.html) 详情。

本节更多是说明micro是由哪些库组成的，以及这些组成micro的库之间的彼此联系。

## 工具集

### API

API模块，它的身份其实就是一个网关或者代理层，它的能力就是让一个单一的入口可以去访问微服务。API工作在我们的软件服务架构的边缘。

<p align="center">
  <img src="images/api.png" />
</p>

### Web

Web模块提供与运行环境可视化交互的能力。未来它也会成为web微服务的一种聚合方式。它包含了代理到web服务app的入口。它会把http请求转成向RPC请求并转发到对应的服务。比如 **/[name]** 就会被路由到注册了的服务上。Web模块会以"go.micro.web."（这个前缀是可以配置的）作为服务名前缀注册，然后相关请求反射代理到它上面。

<p align="center">
  <img src="images/web.png" />
</p>

### Proxy

The proxy is a cli proxy for remote environments.

<p align="center">
  <img src="images/car.png" />
</p>

### Bot

Bot模块可以理解为一个可内嵌在服务平台的小机器人，它可以提供与常见的聊天工具比如Slack、HipChat、XMPP等等进行关于服务状态的交互。它可以通过在聊天工具中发送消息来进行CLI操作。可以根据需要来增加指令完成自定义的任务。

<p align="center">
  <img src="images/bot.png" />
</p>

### CLI

CLI模块很好理解，它是go-micro的命令行工具，提供与实际运行环境的可视化交互的能力。

## Go Micro

Go-micro是独立的RPC框架，它是micro工具集的核心，上面介绍的各个模块组件会基于它来完成工作。下图可以看到每个go-mciro的独立特性。

<p align="center">
  <img src="images/go-micro.png" />
</p>

### Registry

Registry注册模块提供可插拔的服务注册与发现功能，当前实现的方式有Consul，etcd，内存和k8s。即使需求不同，接口也比较容易实现。

### Selector

Selector选择器通过选举提供负载均衡机制。当客户端请求服务时，它首先要向查询到服务注册信息，一般情况是返回所需服务的注册成功的节点列表。选择器会选择其中一个用来提供服务。多次调用选择器会触发均衡算法，当前的算法有round robin（循环调度），哈希随机和黑名单。

### Broker

Broker是事件广播/订阅可插件接口。对于事件驱动的微服务架构，消息广播与订阅得放在首要位置。目前的实现有NATs、rabbitmq、http
(仍在开发中)。

### Transport

Transport传输也是可插拔的点到点消息传输接口，目前的实现有http，rabbitmq，nats。通过提供这种抽象，可以无缝地进行交换传输。

### Client

Client客户端提供发起RPC请求的能力。它集合了注册（registry）、选择器（selector）、broker、传输（transport），当然也具备重试、超时、上下文等等。

### Server

Server服务就是运行了真实的微服务的程序，它提供的服务通过RPC请求的完成。

## Plugins

go-micro的相关插件可以在链接上查看：[micro/go-plugins](https://github.com/micro/go-plugins).

{% include links.html %}
