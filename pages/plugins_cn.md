---
title: 相关插件
keywords: plugins
tags: [plugins]
lang: cn
sidebar: home_sidebar_cn
permalink: plugins_cn.html
summary: 
---

# 相关插件

Micro架构中所有工具都可插拔，也即是说内在实现都是可以剥离出来。

Go-Micro和Micro工具集的插件是各自独立的，可以在导航栏里查看更多的相关信息。

## 几个例子

### Go Micro

- [Etcd Registry](https://github.com/micro/go-plugins/tree/master/registry/etcd) - 基于Etcd服务注册发现插件
- [K8s Registry](https://github.com/micro/go-plugins/tree/master/registry/kubernetes) - 基于k8s的服务注册发现插件
- [Kafka Broker](https://github.com/micro/go-plugins/tree/master/broker/kafka) - Kafka消息代理（broker）

### Micro Toolkit

- [Router](https://github.com/micro/go-plugins/tree/master/micro/router) - 可配置的http路由、代理
- [AWS X-Ray](https://github.com/micro/go-plugins/tree/master/micro/trace/awsxray) - 基于AWS X-Ray集成的链路追踪
- [IP Whitelite](https://github.com/micro/go-plugins/tree/master/micro/ip_whitelist) - IP白名单插件

## 仓库

相关开源的插件可在github中找到[github.com/micro/go-plugins](https://github.com/micro/go-plugins)。

{% include links.html %}
