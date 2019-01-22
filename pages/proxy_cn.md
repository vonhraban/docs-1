---
title: Proxy
keywords: proxy
tags: [proxy]
lang: cn
sidebar: home_sidebar_cn
permalink: proxy_cn.html
summary: 
---

# micro proxy

**micro proxy** 其实就是客户端的代理。

如果服务运行环境不能直接访问，那么就需要通过代理来访问，micro proxy就是来干这事的，它提供http api，这个api可以把客户端的请求转向那些没有直接暴露给客户端的服务。

## 使用方法

### 安装

```shell
go get -u github.com/micro/micro
```

### 依赖

代理基于go-micro开发，也就是说它是依赖服务发现的。

安装Consul

```
brew install consul
consul agent -dev
```

### 运行

Micro代理默认是运行在8081端口下。

启动代理：

```shell
micro proxy
```

### ACME

服务默认使用ACME安全协议

```
MICRO_ENABLE_ACME=true micro proxy
```

可以选择性配置主机白名单

```
MICRO_ENABLE_ACME=true MICRO_ACME_HOSTS=example.com,api.example.com micro proxy 
```

## Proxy CLI

命令行如果要指定代理，可以像下面这样设置：

```shell
MICRO_PROXY_ADDRESS=127.0.0.1:8081 micro list services
```

```
MICRO_PROXY_ADDRESS=127.0.0.1:8081 micro call greeter Say.Hello '{"name": "john"}'
```

{% include links.html %}
