---
title: Micro文档
keywords: homepage
tags: [introduction]
lang: cn
sidebar: home_sidebar_cn
permalink: /cn/index.html
summary: 简化微服务的开发。该文档可以帮助您快速编写新程序。
---

Micro是一套微服务构建工具库。对于微服务架构的应用，Micro提供平台层面、高度弹性的工具组件，让服务开发者们可以把复杂的分布式系统以简单的方式构建起来，并且尽可能让开发者使用最少的时间完成基础架构的构建。

## 开始上手

可以查看[go-micro](https://github.com/micro/go-micro)着手新的服务。

## 概览

最主要部分是微服务工具库：[Micro](https://github.com/micro/micro)。

micro工具库由以下几个部分组成：

- [**`api`**](/cn/api.html) - API Gateway 网关。它是独立的HTTP入口，基于服务发现机制实现动态路由。

- [**`web`**](/cn/web.html) - Web Dashboard web控制台。 提供可视化的发现与管理监控界面。

- [**`cli`**](/cn/cli.html) - Command line interface 命令行接口。提供描述、查询终端服务的交互入口。 

- [**`bot`**](/cn/bot.html) - Slack与hipchat bot消息通知工具。也就是通过消息传递的CLI。

- [**`new`**](/cn/new.html) - 新服务构建模板。

Micro依赖[go-micro](https://github.com/micro/go-micro)，通过它来使其变成可插拨的工具库。

## 相关资源

- [示例](https://github.com/micro/examples)，上面有相关如何使用micro的信息。
- [搜索](https://micro.mu/explore/)搜索可使用的相关micro开源项目。
- [博客](https://micro.mu/blog/)，深入理解micro，了解更多的服务设计思路。
- [视频](https://www.youtube.com/watch?v=xspaDovwk34) 2016年在英国Golong会议上，关于micro的简单介绍。
- [PPT](https://speakerdeck.com/asim)，上面有一些PPT，可供查阅。

## 相关用户

可以查看[用户列表](users.html)

{% include links.html %}
