---
title: 安装指导
keywords: install
tags: [install]
lang: cn
sidebar: home_sidebar_cn
permalink: /cn/install-guide.html
summary: 
---

# 安装指导

## 基础依赖

我们得有服务发现能力，默认使用Consul，可以签出[go-plugins](https://github.com/micro/go-plugins)，里面有连接上Consul的插件。

### Consul

安装Consul：

```shell
brew install consul
consul agent -dev
```

或者使用docker启动：

```shell
docker run consul
```

### 组播DNS（multicast DNS，mDNS）

基于组播DNS，我们可以实现零依赖的服务发现。

当启动参数`--registry=mdns`加到启动命令中，比如：`micro --registry=mdns list services`

## Go Micro

Go Micro是基于Go语言用于开发的微服务的RPC框架。

### 安装Go Micro

```
go get github.com/micro/go-micro
```

### Protobuf

安装**protoc-gen-micro**，用于生成Protobuf的代码。

- [protoc-gen-micro](https://github.com/micro/protoc-gen-micro)

## Toolkit工具集

micro工具集可以辅助操作微服务

### 安装工具集

```
go get github.com/micro/micro
```

### 使用Docker安装

可以通过docker镜像来安装

```
docker pull microhq/micro
```

### 测试 CLI

安装完后可以启动问候（Greeter，用于测试）程序：

```shell
go get github.com/micro/examples/greeter/srv && srv
```

查看服务列表

```shell
$ micro list services
consul
go.micro.srv.greeter
```

查看指定服务信息

```shell
$ micro get service go.micro.srv.greeter
service  go.micro.srv.greeter

version 1.0.0

Id	Address	Port	Metadata
go.micro.srv.greeter-34c55534-368b-11e6-b732-68a86d0d36b6	192.168.1.66	62525	server=rpc,registry=consul,transport=http,broker=http

Endpoint: Say.Hello
Metadata: stream=false

Request: {
	name string
}

Response: {
	msg string
}
```

服务调用

```shell
$ micro call go.micro.srv.greeter Say.Hello '{"name": "John"}'
{
	"msg": "Hello John"
}
```

查看更多信息 [github.com/micro/micro](https://github.com/micro/micro)

{% include links.html %}
