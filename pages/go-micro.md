---
title: Micro Docs
keywords: go-micro
tags: [go-micro]
sidebar: home_sidebar
permalink: go-micro.html
summary: 
---

# Go Micro

Go Micro is a pluggable RPC framework for microservices. It comes with sane defaults but everything can be swapped out. 

<p align="center">
  <img src="images/go-micro.png" />
</p>

## Features

Go Micro abstracts way the details of distributed systems. Here are the main features.

- **Service Discovery** - Applications are automatically registered with service discovery so they can find each other.
- **Load Balancing** - Smart client side load balancing is used to balance requests between instances of a service.
- **Synchronous Communication** - Request-response is provided as a bidirectional streaming transport layer.
- **Asynchronous Communication** - Microservices should promote an event driven architecture. Publish and Subscribe semantics are built in.
- **Message Encoding** - Micro services can encode requests in a number of encoding formats and seamlessly decode based on the Content-Type header.
- **RPC Client/Server** - The client and server leverage the above features and provide a clean simple interface for building microservices.

## Default Plugins

The default plugins are as follows

- Consul or multicast DNS for service discovery
- Random hashed client side load balancing
- JSON-RPC and PROTO-RPC for message encoding
- HTTP for communication

## Examples

An example service can be found in [**examples/service**](https://github.com/micro/examples/tree/master/service). The [**examples**](https://github.com/micro/examples) directory contains many more examples for using things such as middleware/wrappers, selector filters, pub/sub and code generation. 

For the complete greeter example look at [**examples/greeter**](https://github.com/micro/examples/tree/master/greeter). Other examples can be found throughout the GitHub repository.

Check out the blog post to learn how to write go-micro services [https://micro.mu/blog/2016/03/28/go-micro.html](https://micro.mu/blog/2016/03/28/go-micro.html) or watch the talk from the [Golang UK Conf 2016](https://www.youtube.com/watch?v=xspaDovwk34).

## Writing a Service

Check out the [getting started doc](writing-a-go-service.html)

## How it works?

Here are the core components

### Registry

The registry provides a service discovery mechanism to resolve names to addresses. It can be backed by consul, etcd, zookeeper, dns, gossip, etc. 
Services should register using the registry on startup and deregister on shutdown. Services can optionally provide an expiry TTL and reregister 
on an interval to ensure liveness and that the service is cleaned up if it dies.

### Selector

The selector is a load balancing abstraction which builds on the registry. It allows services to be "filtered" using filter functions and "selected" 
using a choice of algorithms such as random, roundrobin, leastconn, etc. The selector is leveraged by the Client when making requests. The client 
will use the selector rather than the registry as it provides that built in mechanism of load balancing. 

### Transport

The transport is the interface for synchronous request/response communication between services. It's akin to the golang net package but provides 
a higher level abstraction which allows us to switch out communication mechanisms e.g http, rabbitmq, websockets, NATS. The transport also 
supports bidirectional streaming. This is powerful for client side push to the server.

### Broker

The broker provides an interface to a message broker for asynchronous pub/sub communication. This is one of the fundamental requirements of an event 
driven architecture and microservices. By default we use an inbox style point to point HTTP system to minimise the number of dependencies required 
to get started. However there are many message broker implementations available in go-plugins e.g RabbitMQ, NATS, NSQ, Google Cloud Pub Sub.

### Codec

The codec is used for encoding and decoding messages before transporting them across the wire. This could be json, protobuf, bson, msgpack, etc. 
Where this differs from most other codecs is that we actually support the RPC format here as well. So we have JSON-RPC, PROTO-RPC, BSON-RPC, etc. 
It separates encoding from the client/server and provides a powerful method for integrating other systems such as gRPC, Vanadium, etc.

### Server

The server is the building block for writing a service. Here you can name your service, register request handlers, add middeware, etc. The service 
builds on the above packages to provide a unified interface for serving requests. The built in server is an RPC system. In the future there maybe 
other implementations. The server also allows you to define multiple codecs to serve different encoded messages.

### Client

The client provides an interface to make requests to services. Again like the server, it builds on the other packages to provide a unified interface 
for finding services by name using the registry, load balancing using the selector, making synchronous requests with the transport and asynchronous 
messaging using the broker. 


The  above components are combined at the top-level of micro as a **Service**.

{% include links.html %}
