---
title: 架构
keywords: architecture,架构
tags: [architecture,架构]
sidebar: home_sidebar_cn
lang: cn
permalink: /cn/architecture.html
summary: 
---

Micro为构建微服务提供了一些基本模板，目标就是要让开发分布式系统尽可能地简化。因为微服务自身就是一个结构，所以micro希望通过工具在逻辑层面上进行职责分离。 

可以查看这篇博客了解micro的[https://micro.mu/blog/2016/04/18/micro-architecture.html](https://micro.mu/blog/2016/04/18/micro-architecture.html) 详情。

本节更多是说明micro是由哪些库组成的，以及这些组成micro的库之间的彼此联系。

## 运行时

### API

API模块，它的身份其实就是一个网关或者代理层，它的能力就是让一个单一的入口可以去访问微服务。API工作在我们的软件服务架构的边缘。

<p align="center">
  <img src="{{site.baseurl}}/images/api.png" />
</p>

### Web

Web模块提供与运行环境可视化交互的能力。未来它也会成为web微服务的一种聚合方式。它包含了代理到web服务app的入口。它会把http请求转成向RPC请求并转发到对应的服务。比如 **/[name]** 就会被路由到注册了的服务上。Web模块会以"go.micro.web."（这个前缀是可以配置的）作为服务名前缀注册，然后相关请求反射代理到它上面。

<p align="center">
  <img src="{{site.baseurl}}/images/web.png" />
</p>

### Proxy

命令行工具远程请求代理

<p align="center">
  <img src="{{site.baseurl}}/images/car.png" />
</p>

### Bot

Bot模块可以理解为一个可内嵌在服务平台的小机器人，它可以提供与常见的聊天工具比如Slack、HipChat、XMPP等等进行关于服务状态的交互。它可以通过在聊天工具中发送消息来进行CLI操作。可以根据需要来增加指令完成自定义的任务。

<p align="center">
  <img src="{{site.baseurl}}/images/bot.png" />
</p>

### CLI

CLI模块很好理解，它是go-micro的命令行工具，提供与实际运行环境的可视化交互的能力。

## Plugins

Go-Micro支持通过插件的方式管理运行时，更多内容可查看[runtime-plugins](runtime-plugins.html)。

{% include links.html %}
