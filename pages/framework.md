---
title: Framework
keywords: go-micro, framework
tags: [go-micro, framework]
sidebar: home_sidebar
permalink: "/framework.html"
summary: Go Micro is a framework for microservices development
---

# Overview

Go Micro provides the core requirements for distributed systems development including RPC and Event driven communication. 
The micro philosophy is sane defaults with a pluggable architecture. We provide defaults to get you started quickly but everything can be easily swapped out.

## Features

Go Micro abstracts away the details of distributed systems. Here are the main features.

- **Service Discovery** - Automatic service registration and name resolution. Service discovery is at the core of micro service
development. When service A needs to speak to service B it needs the location of that service. The default discovery mechanism is
multicast DNS (mdns), a zeroconf system.

- **Load Balancing** - Client side load balancing built on service discovery. Once we have the addresses of any number of instances 
of a service we now need a way to decide which node to route to. We use random hashed load balancing to provide even distribution 
across the services and retry a different node if there's a problem.

- **Message Encoding** - Dynamic message encoding based on content-type. The client and server will use codecs along with content-type 
to seamlessly encode and decode Go types for you. Any variety of messages could be encoded and sent from different clients. The client 
and server handle this by default. This includes proto-rpc and json-rpc by default. 

- **Request/Response** - RPC based request/response with support for bidirectional streaming. We provide an abstraction for synchronous 
communication. A request made to a service will be automatically resolved, load balanced, dialled and streamed. The default 
transport is http/1.1 or http2 when tls is enabled. 

- **Async Messaging** - PubSub is built in as a first class citizen for asynchronous communication and event driven architectures. 
Event notifications are a core pattern in micro service development. The default messaging is point-to-point http/1.1 or http2 when tls 
is enabled. 

- **Pluggable Interfaces** - Go Micro makes use of Go interfaces for each distributed system abstraction. Because of this these interfaces 
are pluggable and allows Go Micro to be runtime agnostic. You can plugin any underlying technology. Find plugins in 
[github.com/micro/go-plugins](https://github.com/micro/go-plugins).

## Getting started

- [Dependencies](#dependencies)
  * [Protobuf](#protobuf)
  * [Discovery](#discovery)
- [Installation](#installation)
- [Writing a Service](#writing-a-service)
- [Publish & Subscribe](#publish--subscribe)
- [Plugins](#plugins)
- [Wrappers](#wrappers)
- [Examples](#examples)

## Dependencies

Go Micro makes use of protobuf by default. This is so we can code generate boilerplate code and also provide 
an efficient wire format for transferring data back and forth between services.

We also require some form of service discovery to resolve service names to their addresses along with 
metadata and endpoint information. See below for more info.

### Protobuf

You'll need to install protobuf to code generate API interfaces:

- [protoc-gen-micro](https://github.com/micro/protoc-gen-micro)

### Discovery

Service discovery is used to resolve service names to addresses. By default we provide a zeroconf discovery system 
using multicast DNS. This comes built-in on most operating systems. If you need something more resilient and multi-host then use etcd.

### Etcd

[Etcd](https://github.com/etcd-io/etcd) can be used as an alternative service discovery system.

- Download and run [etcd](https://github.com/etcd-io/etcd)
- Pass `--registry=etcd` to any command or the enviroment variable `MICRO_REGISTRY=etcd`

```
MICRO_REGISTRY=etcd go run main.go
```

Discovery is pluggable. Find plugins for consul, kubernetes, zookeeper and more in the [micro/go-plugins](https://github.com/micro/go-plugins) repo.

## Installation

Go Micro is a framework for Go based development. You can easily get this with the go toolchain.


Import go-micro in your service

```
import "github.com/micro/go-micro"
```

We provide release tags and would recommend to stick with the latest stable releases. Making use of go modules will enable this.

```
# enable go modules
export GO111MODULE=on
# initialise go modules in your app
go mod init
# now go get
go get ./...
```

## Writing a service

Writing a service is incredibly simple with Go Micro. It provides you the framework to move fast without needing to understand 
everything at first. Below is a simple greeter service example that we'll run through.

Find the full example code at [examples/service](https://github.com/micro/examples/tree/master/service).

### Service Proto

One of the key requirements of microservices is strongly defined interfaces. Micro uses protobuf to achieve this.

Here we define the Greeter handler with the method Hello. It takes a Request and Response both with one string arguments.

```proto
syntax = "proto3";

service Greeter {
	rpc Hello(Request) returns (Response) {}
}

message Request {
	string name = 1;
}

message Response {
	string greeting = 2;
}
```

### Generate Proto

After writing the proto definition we must compile it using protoc with the micro plugin.

```shell
protoc --proto_path=$GOPATH/src:. --micro_out=. --go_out=. path/to/greeter.proto
```

### Implement Service

Now that we've defined the service interface we need to implement the service.

Below is the code for the greeter service. It does the following:

1. Implements the interface defined for the Greeter handler
2. Initialises a [micro.Service](https://godoc.org/github.com/micro/go-micro#Service)
3. Registers the Greeter handler
4. Runs the service

```go
package main

import (
	"context"
	"fmt"

	micro "github.com/micro/go-micro"
	proto "github.com/micro/examples/service/proto"
)

type Greeter struct{}

func (g *Greeter) Hello(ctx context.Context, req *proto.Request, rsp *proto.Response) error {
	rsp.Greeting = "Hello " + req.Name
	return nil
}

func main() {
	// Create a new service. Optionally include some options here.
	service := micro.NewService(
		micro.Name("greeter"),
	)

	// Init will parse the command line flags.
	service.Init()

	// Register handler
	proto.RegisterGreeterHandler(service.Server(), new(Greeter))

	// Run the server
	if err := service.Run(); err != nil {
		fmt.Println(err)
	}
}
```

### Run Service

Now run the example using Go. If you're working with the example code do the following:

```
go run examples/service/main.go
```

This should output something like the folloing

```
2019-11-13 21:39:38.327452 I | Transport [http] Listening on [::]:32945
2019-11-13 21:39:38.327548 I | Broker [http] Connected to [::]:38955
2019-11-13 21:39:38.328095 I | Registry [mdns] Registering node: greeter-39373107-5ae7-42a2-b5e2-ebebf7eafbd9
```

### Write Client

Once we've got a service we actually need a way to query it. This is at the heart of microservices as we not 
only serve but also consume other services. Below is the client code to query the greeter service. 

The generated proto includes a greeter client to reduce boilerplate code.

```go
package main

import (
	"context"
	"fmt"

	micro "github.com/micro/go-micro"
	proto "github.com/micro/examples/service/proto"
)

func main() {
	// Create a new service
	service := micro.NewService(micro.Name("greeter.client"))
	// Initialise the client and parse command line flags
	service.Init()

	// Create new greeter client
	greeter := proto.NewGreeterService("greeter", service.Client())

	// Call the greeter
	rsp, err := greeter.Hello(context.TODO(), &proto.Request{Name: "John"})
	if err != nil {
		fmt.Println(err)
	}

	// Print response
	fmt.Println(rsp.Greeting)
}
```

Now run the client

```shell
go run client.go
```

And the output should simply print the response

```
Hello John
```

## Publish & Subscribe

Microservices is an event driven architecture patterna and so Go Micro builds in the concept of asynchronous messaging 
using a message broker interface. It seamlessly operates on protobuf types for you. Automatically encoding and decoding 
messages as they are sent and received from the broker.

By default go-micro includes a point-to-point http broker but this can be swapped out via [go-plugins](https://github.com/micro/go-plugins).

### Publish Message

Create a new publisher with a `topic` name and service client

```go
p := micro.NewPublisher("events", service.Client())
```

Publish a proto message

```go
p.Publish(context.TODO(), &proto.Event{Name: "event"})
```

### Subscribe

Create a message handler. It's signature should be `func(context.Context, v interface{}) error`.

```go
func ProcessEvent(ctx context.Context, event *proto.Event) error {
	fmt.Printf("Got event %+v\n", event)
	return nil
}
```

Register the message handler with a `topic`

```go
micro.RegisterSubscriber("events", ProcessEvent)
```

See [examples/pubsub](https://github.com/micro/examples/tree/master/pubsub) for a complete example.

## Plugins

By default go-micro only provides a few implementation of each interface at the core but it's completely pluggable. 
There's already dozens of plugins which are available at [github.com/micro/go-plugins](https://github.com/micro/go-plugins). 
Contributions are welcome! Plugins ensure that your Go Micro services live on long after technology evolves.

### Add plugins

If you want to integrate plugins simply link them in a separate file and rebuild

Create a plugins.go file and import the plugins you want:

```go
package main

import (
        // consul registry
        _ "github.com/micro/go-plugins/registry/consul"
        // rabbitmq transport
        _ "github.com/micro/go-plugins/transport/rabbitmq"
        // kafka broker
        _ "github.com/micro/go-plugins/broker/kafka"
)
```

Build your application by including the plugins file:

```shell
# assuming files main.go and plugins.go are in the top level dir
 
# For local use
go build -o service *.go
```

Flag usage of plugins:

```shell
service --registry=etcdv3 --transport=nats --broker=kafka
```

Or what's preferred is using environment variables for 12-factor apps

```
MICRO_REGISTRY=consul \
MICRO_TRANSPORT=rabbitmq \
MICRO_BROKER=kafka \
service
```

### Plugin Option

Alternatively you can set the plugin as an option to a service directly in code

```go
package main

import (
        "github.com/micro/go-micro" 
        // consul registry
        "github.com/micro/go-plugins/registry/consul"
        // rabbitmq transport
        "github.com/micro/go-plugins/transport/rabbitmq"
        // kafka broker
        "github.com/micro/go-plugins/broker/kafka"
)

func main() {
	registry := consul.NewRegistry()
	broker := kafka.NewBroker()
	transport := rabbimq.NewTransport()

        service := micro.NewService(
                micro.Name("greeter"),
                micro.Registry(registry),
                micro.Broker(broker),
                micro.Transport(transport),
        )

	service.Init()
	service.Run()
}
```

### Write Plugins

Plugins are a concept built on Go's interface. Each package maintains a high level interface abstraction. 
Simply implement the interface and pass it in as an option to the service.

The service discovery interface is called [Registry](https://godoc.org/github.com/micro/go-micro/registry#Registry). 
Anything which implements this interface can be used as a registry. The same applies to the other packages.

```go
type Registry interface {
    Register(*Service, ...RegisterOption) error
    Deregister(*Service) error
    GetService(string) ([]*Service, error)
    ListServices() ([]*Service, error)
    Watch() (Watcher, error)
    String() string
}
```

Browse [go-plugins](https://github.com/micro/go-plugins) to get a better idea of implementation details.

## Wrappers

Go Micro includes the notion of middleware as wrappers to extend the functionality of your code and integrate 
external requirements. The client or handlers can be wrapped using what's known as the decorator pattern.

### Handler

Here's an example service handler wrapper which logs the incoming request

```go
// implements the server.HandlerWrapper
func logWrapper(fn server.HandlerFunc) server.HandlerFunc {
	return func(ctx context.Context, req server.Request, rsp interface{}) error {
		fmt.Printf("[%v] server request: %s", time.Now(), req.Endpoint())
		return fn(ctx, req, rsp)
	}
}
```

It can be initialised when creating the service

```go
service := micro.NewService(
	micro.Name("greeter"),
	// wrap the handler
	micro.WrapHandler(logWrapper),
)
```

### Client

Here's an example of a client wrapper which logs requests made

```go
type logWrapper struct {
	client.Client
}

func (l *logWrapper) Call(ctx context.Context, req client.Request, rsp interface{}, opts ...client.CallOption) error {
	fmt.Printf("[wrapper] client request to service: %s endpoint: %s\n", req.Service(), req.Endpoint())
	return l.Client.Call(ctx, req, rsp)
}

// implements client.Wrapper as logWrapper
func logWrap(c client.Client) client.Client {
	return &logWrapper{c}
}
```

It can be initialised when creating the service

```go
service := micro.NewService(
	micro.Name("greeter"),
	// wrap the client
	micro.WrapClient(logWrap),
)
```

## Examples

The example service for the above guide can be found in [**examples/service**](https://github.com/micro/examples/tree/master/service).

Further examples exist in the [**examples**](https://github.com/micro/examples) directory for using things such as middleware/wrappers, 
selector filters, pub/sub, grpc, plugins and much more. For the complete greeter example look at 
[**examples/greeter**](https://github.com/micro/examples/tree/master/greeter).

{% include links.html %}
