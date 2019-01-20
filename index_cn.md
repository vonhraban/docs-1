---
title: Micro文档
keywords: homepage
tags: [introduction]
lang: cn
sidebar: home_sidebar_cn
permalink: index_cn.html
summary: 简化微服务的开发。该文档可以帮助您快速编写新程序。
---

Micro 旨在提供构建微服务系统所需的关键组件。Micro拥有微服务架构，并且它也是一组构建可伸缩平台的工具组件。Micro尝试着把分布式系统的复杂度封装起来，尽可能让以便让开发者秒懂。

## 开始上手

可以查看[go-micro](https://github.com/micro/go-micro)着手新的服务。

## 概览

最主要部分是微服务工具库：[Micro](https://github.com/micro/micro)。

micro工具库由以下几个部分组成：

- [**`api`**](api.html) - API Gateway 网关。它是独立的HTTP入口，基于服务发现机制实现动态路由。

- [**`web`**](web.html) - Web Dashboard web控制台。 提供可视化的发现与管理监控界面。

- [**`cli`**](cli.html) - Command line interface 命令行接口。提供描述、查询终端服务的交互入口。 

- [**`bot`**](bot.html) - Slack与hipchat bot消息通知工具。也就是通过消息传递的CLI。

- [**`new`**](new.html) - 新服务构建模板。

Micro依赖[go-micro](https://github.com/micro/go-micro)，通过它来使其变成可插拨的工具库。

## 相关资源

- [示例](https://github.com/micro/examples)，上面有相关如何使用micro的信息。
- [搜索](https://micro.mu/explore/)搜索可使用的相关micro开源项目。
- [博客](https://micro.mu/blog/)，深入理解micro，了解更多的服务设计思路。
- [视频](https://www.youtube.com/watch?v=xspaDovwk34) 2016年在英国Golong会议上，关于micro的简单介绍。
- [PPT](https://speakerdeck.com/asim)，上面有一些PPT，可供查阅。

## 捐献者

[捐献者](https://micro.mu/#sponsors)

## 相关用户

可以查看[用户列表](users.html)

{% include links.html %}
