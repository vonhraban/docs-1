---
title: Install Guide
keywords: install
tags: [install]
sidebar: home_sidebar
permalink: install-guide.html
summary: 
---

# Install Guide

## Dependencies

We use service discovery to locate services. The default is multicast DNS so a zeroconf configuration. If you needs something multi-host and 
more resilient use consul. Checkout [go-plugins](https://github.com/micro/go-plugins) to try something else.

### Consul

If using consul

```shell
brew install consul
consul agent -dev
```

Or

```shell
docker run consul
```

Pass `--registry=consul` to any commands e.g `micro --registry=consul list services`

## Go Micro

Go Micro is an RPC framework for development microservices in Go

### Install

```
go get github.com/micro/go-micro
```

### Protobuf

You will also need protoc-gen-micro for code generation

- [protoc-gen-micro](https://github.com/micro/protoc-gen-micro)

## Toolkit

The micro toolkit provides various ways to access microservices

### Install

```
go get github.com/micro/micro
```

### Docker

Prebuilt docker images are available

```
docker pull microhq/micro
```

### Try CLI

Run the greeter service

```shell
go get github.com/micro/examples/greeter/srv && srv
```

List services

```shell
$ micro list services
consul
go.micro.srv.greeter
```

Get Service

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

Call service

```shell
$ micro call go.micro.srv.greeter Say.Hello '{"name": "John"}'
{
	"msg": "Hello John"
}
```

Visit [github.com/micro/micro](https://github.com/micro/micro) to learn more

{% include links.html %}
