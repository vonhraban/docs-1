---
title: Plugins
keywords: plugins
tags: [plugins]
sidebar: home_sidebar
permalink: plugins.html
summary: 
---

# Plugins

Micro provides a pluggable architecture for all tooling. This means underlying implementations can be swapped out.

Go-Micro and the Micro toolkit include separate types of plugins. Navigate from the sidebar to learn more about each.

## Examples

### Go Micro

- [Etcd Registry](https://github.com/micro/go-plugins/tree/master/registry/etcd) - Service discovery using Etcd
- [K8s Registry](https://github.com/micro/go-plugins/tree/master/registry/kubernetes) - Service discovery using Kubernetes
- [Kafka Broker](https://github.com/micro/go-plugins/tree/master/broker/kafka) - Kafka message broker

### Toolkit

- [Router](https://github.com/micro/go-plugins/tree/master/micro/router) - Configurable http routing and proxying
- [AWS X-Ray](https://github.com/micro/go-plugins/tree/master/micro/trace/awsxray) - Tracing integration for AWS X-Ray
- [IP Whitelite](https://github.com/micro/go-plugins/tree/master/micro/ip_whitelist) - Whitelisting IP address access

## Repository

The open source plugins can be found at [github.com/micro/go-plugins](https://github.com/micro/go-plugins).


{% include links.html %}
