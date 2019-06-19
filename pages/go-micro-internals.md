---
title: Internals
keywords: go-micro
tags: [go-micro]
sidebar: home_sidebar
permalink: /go-micro-internals.html
summary: A description of the go-micro internals
---

Go Micro is a pluggable RPC framework. It's used for distributed systems development.

<p align="center">
  <img src="images/go-micro.svg" />
</p>

## Features

Go Micro abstracts away the details of distributed systems. Here are the main features.

- **Service Discovery** - Automatic registration and name resolution with service discovery
- **Load Balancing** - Smart client side load balancing of services built on discovery
- **Synchronous Comms** - RPC based communication with support for bidirectional streaming
- **Asynchronous Comms** - PubSub interface built in for event driven architectures
- **Message Encoding** - Dynamic encoding based on content-type with protobuf and json out of the box
- **Service Interface** - All features are packaged in a simple high level interface for developing microservices

Go Micro supports both the Service and Function programming models. Read on to learn more.

## Plugins

Go-micro makes use of the Go interface for it's abstractions. Because of this the underlying implementation can be swapped out.

We provide sane defaults out of the box.

- mDNS, gossip or consul for service discovery
- Random hashed client side load balancing
- JSON-RPC 1.0 and PROTO-RPC for message encoding
- HTTP for communication

## Examples


An example service can be found in [**examples/service**](https://github.com/micro/examples/tree/master/service) and function in [**examples/function**](https://github.com/micro/examples/tree/master/function). 

The [**examples**](https://github.com/micro/examples) directory contains examples for using things such as middleware/wrappers, selector filters, pub/su
b, grpc, plugins and much more. For the complete greeter example look at [**examples/greeter**](https://github.com/micro/examples/tree/master/greeter). 
Other examples can be found throughout the GitHub repository.

Watch the [Golang UK Conf 2016](https://www.youtube.com/watch?v=xspaDovwk34) video for a high level overview.

## Packages

Go micro is composed of a number of packages. 

- **transport** for sync messaging
- **broker** for async messaging
- **codec** for message encoding
- **registry** for service discovery
- **selector** for load balancing
- **client** for making requests
- **server** for handling requests

Further details are below

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

## Internals

Below is an explanation of inner workings of core functions

### service.Run()

A go-micro service is started by calling service.Run()

1. Execute before start functions


        for _, fn := range s.opts.BeforeStart {
                if err := fn(); err != nil {
                        return err
                }
        }


2. Start the server


        if err := s.opts.Server.Start(); err != nil {
                return err
        }


3. Register with service discovery

        if err := s.opts.Server.Register(); err != nil {
                return err
        }


 4. Execute after start functions

        for _, fn := range s.opts.AfterStart {
                if err := fn(); err != nil {
                        return err
                }
        }

### server.Start()

server.Start is called by service.Run

1. Call transport.Listen to listen for connections

        ts, err := config.Transport.Listen(config.Address)
        if err != nil {
                return err
        }

2. Call transport.Accept to start accepting connections

        go ts.Accept(s.accept)

3. Call broker.Connect to start connect to the message broker
        
        config.Broker.Connect()

4. Wait an exit signal, close the transport and disconnect the broker

        go func() {
                // wait for exit
                ch := <-s.exit

                // wait for requests to finish
                if wait(s.opts.Context) {
                        s.wg.Wait()
                }

                // close transport listener
                ch <- ts.Close()

                // disconnect the broker
                config.Broker.Disconnect()
        }()

## Writing a Service

Check out the [getting started doc](writing-a-go-service.html)

{% include links.html %}
