---
title: Go gRPC支持
keywords: go-grpc
tags: [go-grpc]
lang: cn
sidebar: home_sidebar_cn
permalink: "/cn/go-grpc.html"
summary: 
---

Micro gRPC是micro的gRPC框架插件，简化开发基于gRPC的服务。

## 概览

micro提供有基于Go的gRPC插件[go-micro](https://github.com/micro/go-micro)，该插件可以在内部集成gPRC，并与之无缝交互，让开发gRPC更简单，并支持[grpc-gatewa](https://github.com/grpc-ecosystem/grpc-gateway)。

micro有面向gRPC的[客户端](https://github.com/micro/go-plugins/tree/master/client)和[服务端](https://github.com/micro/go-plugins/tree/master/server)插件，go-grpc库调用客户端/服务端插件生成micro需要的gRPC代码，而客户端/服务端插件都是从[github.com/grpc/grpc-go](https://github.com/grpc/grpc-go)扩展而来，也即是说，我们不需要去知道go-micro是如何编解码或传输节点就可以使用原生的gRPC。

<img src="../images/go-grpc.png" />

## 示例

我们提供了一个简单的节点Greeter问候程序来演示对gRPC的支持：[examples/greeter](https://github.com/micro/go-grpc/tree/cn-lang/examples/greeter)。

## 安装protobuf

我们需要使用Protobuf来生成代码，请根据您的环境参考[protobuf](https://github.com/protocolbuffers/protobuf)安装。

安装好protobuf后，还需要安装
- [protoc-gen-micro](https://github.com/micro/protoc-gen-micro)

## 服务发现

服务发现负责把服务名到服务所在地址。

### Consul

micro默认使用[Consul](https://www.consul.io/) 作为服务发现的注册中心。您可以查考[install guide](https://www.consul.io/intro/getting-started/install.html)安装。

发现机制是可插拔的，我们目前支持使用etcd、kubernetes、zookeeper等等，具体详见[micro/go-plugins](https://github.com/micro/go-plugins)。

### mDNS

[组播，Multicast DNS](https://en.wikipedia.org/wiki/Multicast_DNS)已经内置在micro中，mDNS不需要依赖任何注册中心，可以在一般情况下的局域网中使用。

在micro服务的任意启动指令中传入`--registry=mdns`或者声明环境变量`MICRO_REGISTRY=mdns`也可，比如：

```
MICRO_REGISTRY=mdns go run main.go
```

## 编写服务

Go-grpc服务与go-micro服务一样，也就是说你可以直接将服务声明方式`micro.NewService`换成`grpc.NewService`，而不需要改动其它代码。

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

## 使用Micro

您可能需要使用micro的工具集编写grpc服务，micro支持使用工具集预构建或者简单把grpc的客户端插件导入项目中重新编译构建即可。

### 安装micro

```
go get github.com/micro/go-grpc/cmd/micro
```

### 或者自行编译

```
go get github.com/micro/micro
```

在main.go目录创建plugins.go文件，专门引入插件
```go
package main

import _ "github.com/micro/go-plugins/client/grpc"
import _ "github.com/micro/go-plugins/server/grpc"
```

重新编译二进制
```shell
// 本地使用
go build -i -o micro ./main.go ./plugins.go
```

使用插件
```shell
micro --client=grpc --server=grpc
```

## gRPC网关

Go-grpc与gRPC生态是无缝集成的，所以grpc-gateway使用没特别之处。

我们提供了简单的问候Greeter服务，可以参考[examples/grpc/gateway](https://github.com/micro/examples/tree/master/grpc/gateway)。

{% include links.html %}
