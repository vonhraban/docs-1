---
title: Install Guide
keywords: install
tags: [install]
sidebar: home_sidebar
permalink: install-guide.html
summary: 
---

## Framework

Go Micro is an RPC framework for development microservices in Go

### Dependencies

You will need protoc-gen-micro for code generation

- [protoc-gen-micro](https://github.com/micro/protoc-gen-micro)

### Install

```
go get github.com/micro/go-micro
```

## Runtime

Micro provides a runtime for accessing and managing microservices

### Install

From source

```
go get github.com/micro/micro
```

Docker image

```
docker pull micro/micro
```

Latest release binaries

```
# Mac OS or Linux
curl -fsSL https://micro.mu/install.sh | /bin/bash

# Windows
powershell -Command "iwr -useb https://micro.mu/install.ps1 | iex"
```

### Try CLI

Run the greeter service

```shell
go get github.com/micro/examples/greeter/srv && srv
```

List services

```shell
micro list services
```

Get Service

```shell
micro get service go.micro.srv.greeter
```

Output

```shell
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
micro call go.micro.srv.greeter Say.Hello '{"name": "John"}'
```

Output

```shell
{
	"msg": "Hello John"
}
```

Visit [github.com/micro/micro](https://github.com/micro/micro) to learn more

{% include links.html %}
