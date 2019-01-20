---
title: 常见问题
keywords: faq,常见问题
tags: [faq,常见问题]
lang: cn
sidebar: home_sidebar_cn
permalink: faq_cn.html
summary:
---

FAQ章节包含了的常见问题，并作了一些解答。

## 什么是 Micro ？

Micro 是一个着眼于分布式系统开发的微服务生态系统。

- Micro 是[框架](https://github.com/micro/go-micro)
- Micro 也是[工具集](https://github.com/micro/micro)
- Micro 有[社区](http://slack.micro.mu/)
- Micro 还是一个[生态系统](https://micro.mu/explore/)

### Micro是开源的

Micro由开源的库与工具组成，旨在辅助微服务开发。

- **go-micro** - 基于Go语言的可插拔RPC微服务开发框架；包含服务发现、RPC客户/服务端、广播/订阅机制等等。
- **go-plugins** - go-micro的插件有etcd、kubernetes、nats、rabbitmq、grpc等等。
- **micro** - 微服务工具集包含传统的入口点（entry point）；API 网关、CLI、Slack Bot、代理及Web UI。

还有其它相关的库和服务可以参考 [github.com/micro](https://github.com/micro)。

### 社区

我们有上千人的社区在Slack平台上：[slack.micro.mu](http://slack.micro.mu/)；

### 生态系统

Micro跨过了单一组合开发的模式，她的开源工具与服务都是由社区贡献的。

相关的生态系统可以进右边的传送门查看：[micro.mu/explore/](https://micro.mu/explore/).

## 该从何入手

可以先从[go-micro](https://github.com/micro/go-micro)入手。 readme文档里提供了一个demo微服务示例。

如果要了解更多的资源可以查阅[开始入门](https://micro.mu/docs/writing-a-go-service.html) ，或者签出[示例](https://github.com/micro/examples)源码。

可以通过[micro](https://github.com/micro/micro)工具集的cli，web ui，slack，或者api网关（api gateway）来访问操控务。

## micro有哪些使用者

[相关用户](https://micro.mu/docs/users.html)页面列出了一些使用micro服务的公司（列表更新可能不会太及时）。 

还有很多公司在使用我们的框架但是没有公开列出来，如果你们你们正在使用micro，可以告诉我们，我们会列出来。

## 如何才能使用micro

其实非常简单

1. 使用[go-micro](https://github.com/micro/go-micro)编写服务。
2. 使用[micro](https://github.com/micro/micro)工具集来访问这些服务。

你可以签出我们在github上的简易[Greeter](https://github.com/micro/examples/tree/master/greeter)示例。

## 除了Consul，可以使用其它的注册中心吗

当然是可以的，服务的注册发现的实现机制是可插拔的，之所以使用Consul是因为它拥有的特性以及它足够简单。
比如：

### Etcd

如果你想使用etcd那你只需要引用etcd包，然后在启动的注册方式上标明使用的是etcd就行了。

```go
import (
        _ "github.com/micro/go-plugins/registry/etcd"
)
```

```shell
service --registry=etcd --registry_address=127.0.0.1:2379
```

### 零依赖

micro专门为零依赖配置内置有一个多路广播DNS服务注册中心。

如果要使用，只需要在程序启动指令上传上`--registry=mdns`或者`MICRO_REGISTRY=mdns`。

## Micro可以在哪些环境运行

micro对运行环境不挑食。只要你喜欢，在哪都行，裸机, 亚马逊AWS或者Google Cloud，也可以运行在你喜欢的容器编排系统中比如：Mesos、Kubernetes。

右边的这个链接中有关于如何使用K8s来开发micro服务的[micro kubernetes demo](https://github.com/micro/kubernetes)

## API、Web、SRV 服务之间的区别是什么

<img src="images/arch.png" />

API、Web、SRV作为micro工具集的一部分，我们尝试着使用一些设计模式规划出弹性的架构，通过把**API接口**、**Web管理控制台**、**SRV**（SRV是service的简称，可理解为后台业务逻辑部分）。

### API 服务

micro api默认把**go.micro.api**作为API网关服务的命名空间，micro api遵从API网关模式。

查看更多内容请求翻阅：[api](https://github.com/micro/micro/tree/master/api)

### Web 服务

Web服务由micro web提供，默认命名空间是**go.micro.web**。我们坚信，web应用作为微服务中的一等公民，所以把构建web管理控制台作为微服务的一部分显得非常重要。micro web其实是一个反向代理，并把http请求基于路径解析到适当的web应用程序。

查看更多内容请求翻阅：[web](https://github.com/micro/micro/tree/master/web)

### SRV 服务

SRV 服务是RPC服务的基础，也就是你常写的服务类型。我们一般把它称作RPC或后后端服务，因为它作为后台架构的一部分，不应该暴露在最外层。默认情况下，我们使用**go.micro.srv**作为它的命名空间，或者你可以使用像**com.example.srv**这样的名字。

## Micro的性能如何

性能目前并不是Micro主要关注的事情。虽然代码写出来应该是最优化并且要避免超过负载，但是确实是没有这么多时间花在基准压力测试上。与net/http或者其它web框架相互比较目前毫无意义。Micro提供的是更高级别的微服务需求，这里面就包含服务发现、负载均衡、消息编码等等。要比较，我想得把这些都一起比较了才行。

如果你还是关心性能问题，最简单的方式就是通过传递以下参数运行程序，便可以提取出最大值。

```
--selector=cache # 激活基于内存的服务结点发现机制
--client_pool_size=10 # 激活客户端连接池
```

## Micro支持gRPC吗

支持。这儿有几个插件：transport, client and server。

可以查看[micro/go-plugins](https://github.com/micro/go-plugins). 

我们也提供了golang版本的gRPC快速上手demo：[micro/go-grpc](https://github.com/micro/go-grpc).

## Micro与Go-Kit比较

这个问题经常出现，那二者的区别有哪些呢？

Go-kit声称自己是一个微服务的标准库。像GO一样，go-kit提供独立的包，通过这些包，开发者可以用来组建自己的应用程序。Go-kit非常不错，基于Go-kit，你可以完全掌控你定义的服务。

Go-micro则是一个面向微服务的可插拔RPC框架。go-micro是一个只在特殊方向上努力的框架，它尝试简化分布式系统之间的通信，所以我们可以花更多的时间在我们需要关注的业务逻辑上。对于想快速启动，把程序跑起来，同时用拥有一些可插拔的能力从基础架构中断开的能力，而不用修改代码，那么go-micro也很不错。

Micro作为一个微服务工具库，好比一把瑞士军刀，在我们构建微服务时，可以提供传统的接入点，比如http api gateway，web ui，cli，slack bot等等。Micro使用工具来引导架构关注点之间逻辑上的隔离，推动开发者创建API层的服务来暴露对外的API接口，并且创建隔离于对外API的Web层微服务。

如果想全盘掌控，那么使用go-kit；但是如果想弄一个有想法框架，使用go-micro。

## Where Can I Learn More?

- slack社区 - [slack.micro.mu](http://slack.micro.mu)
- 博客 - [micro.mu/blog](https://micro.mu/blog)
- 联系我们 - [contact@micro.mu](mailto:contact@micro.mu)

{% include links.html %}
