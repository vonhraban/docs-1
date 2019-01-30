---
title: Go gRPC
keywords: go-grpc
tags: [go-grpc]
sidebar: home_sidebar
permalink: "/go-grpc.html"
summary: 
---

A micro gRPC framework. A simplified experience for building gRPC services.

## Overview

Go gRPC makes use of [go-micro](https://github.com/micro/go-micro) plugins to create a simpler framework for gRPC development. It interoperates with 
standard gRPC services seamlessly, including the [grpc-gateway](https://github.com/grpc-ecosystem/grpc-gateway). The go-grpc library uses 
the go-micro client and server plugins which make use of [github.com/grpc/grpc-go](https://github.com/grpc/grpc-go). This means we ignore 
the go-micro codec and transport but you get a native grpc experience.

<img src="images/go-grpc.svg" />

## Examples

Find an example greeter service in [examples/greeter](https://github.com/micro/go-grpc/tree/master/examples/greeter).

## Getting Started

- [Install Protobuf](#install-protobuf)
- [Service Discovery](#service-discovery)
- [Writing a Service](#writing-a-service)
- [Using with Micro](#use-with-micro)
- [Using with gRPC Gateway](#use-with-grpc-gateway)


## Install Protobuf

Protobuf is required for code generation

You'll need to install:

- [protoc-gen-micro](https://github.com/micro/protoc-gen-micro)

## Service Discovery

Service discovery is used to resolve service names to addresses. Multicast DNS is the default, a zeroconf system. If you need something more resilient and multi-host use consul.

### Consul

[Consul](https://www.consul.io/) is used as the default service discovery system. See the [install guide](https://www.consul.io/intro/getting-started/install.html).

Discovery is pluggable. Find plugins for etcd, kubernetes, zookeeper and more in the [micro/go-plugins](https://github.com/micro/go-plugins) repo.

Pass `--registry=consul` to any command or the enviroment variable `MICRO_REGISTRY=consul`

```
MICRO_REGISTRY=consul go run main.go
```

## Writing a Service

Go-grpc service is identical to a go-micro service. Which means you can swap out `micro.NewService` for `grpc.NewService` 
with zero other code changes.

```go
package main

import (
	"context"
	"time"

	"github.com/micro/go-grpc"
	"github.com/micro/go-micro"
	hello "github.com/micro/go-grpc/examples/greeter/server/proto/hello"
)

type Say struct{}

func (s *Say) Hello(ctx context.Context, req *hello.Request, rsp *hello.Response) error {
	rsp.Msg = "Hello " + req.Name
	return nil
}

func main() {
	service := grpc.NewService(
		micro.Name("greeter"),
	)

	service.Init()

	hello.RegisterSayHandler(service.Server(), new(Say))

	if err := service.Run(); err != nil {
		log.Fatal(err)
	}
}
```

## Use with Micro

You may want to use the micro toolkit with grpc services. To do this either use the prebuilt toolkit or 
simply include the grpc client plugin and rebuild the toolkit.

### Go Get

```
go get github.com/micro/go-grpc/cmd/micro
```

### Build Yourself

```
go get github.com/micro/micro
```

Create a plugins.go file
```go
package main

import _ "github.com/micro/go-plugins/client/grpc"
import _ "github.com/micro/go-plugins/server/grpc"
```

Build binary
```shell
// For local use
go build -i -o micro ./main.go ./plugins.go
```

Flag usage of plugins
```shell
micro --client=grpc --server=grpc
```

## Use with gRPC Gateway

Go-grpc seamlessly integrates with the gRPC ecosystem. This means the grpc-gateway can be used as per usual.

Find an example greeter api at [examples/grpc/gateway](https://github.com/micro/examples/tree/master/grpc/gateway).

{% include links.html %}
