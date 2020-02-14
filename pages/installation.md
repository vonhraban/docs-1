---
title: Install Guide
keywords: install
tags: [install]
sidebar: home_sidebar
permalink: installation.html
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
go get github.com/micro/micro/v2
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

### Usage

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

version 2019.11.09.10.34

ID      Address Metadata
go.micro.srv.greeter-e25a5edd-0936-4d32-b4d7-e62bf454d5f7       172.17.0.1:33031        broker=http,protocol=mucp,registry=mdns,server=mucp,transport=http

Endpoint: Say.Hello

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

{% include links.html %}
