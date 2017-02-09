---
title: Micro Docs
keywords: features
tags: [features]
sidebar: home_sidebar
permalink: features.html
summary: 
---

# Features

The micro ecosystem is rapidly growing but there's still a lot to do.

The features page keeps track of the most important or noteworthy features.

## Micro

### [API](https://github.com/micro/micro/tree/master/api)
  * [x] Allow requests directly to RPC services via path (/greeter/say/hello => service: greeter method: Say.Hello)
  * [x] TLS Support
  * [x] Allow namespace to be set via flags
  * [x] Apache log format
  * [x] Stats page
  * [x] Allow REST requests to RPC based services
  * [x] Make the choice a flag/env var
  * [x] HTTP Middleware/Plugins
  * [x] Configurable namespace for services
  * [ ] Configurable hostnames
  * [ ] Support label based routing
  * [ ] Support weighted load balancing

### [Web](https://github.com/micro/micro/tree/master/web)
  * [x] Proxy requests to "web" micro services
  * [x] List "web" micro services on home screen
  * [x] TLS Support
  * [x] Web Sockets
  * [x] Allow namespace to be set via flags
  * [x] Apache log format
  * [x] Stats page
  * [x] CLI interface in Web UI
  * [ ] Configurable hostnames

### [Sidecar](https://github.com/micro/micro/tree/master/car)
  * [x] TLS Support
  * [x] Apache log format
  * [x] Stats page
  * [ ] Raise awareness for non Go native app usage
  * [ ] Make it work with multiple transports

### [CLI](https://github.com/micro/micro/tree/master/cli)
  * [x] Support querying via proxying the sidecar
  * [x] Allow connecting through the API or Web where private network isn't available
    - Done via the [Sidecar](https://github.com/micro/micro/tree/master/car#proxy-cli-requests)
### Bot
  * [x] Implement the bot
  * [x] Feature parity with CLI
  * [x] Slack input
  * [x] Hipchat input
  * [ ] IMAP input
  * [ ] Broker input
  * [ ] Stream interface

### Dependencies
  * [ ] Create dependency management config for services
  * [ ] Allow push/pull from micro.mu

### Examples
  * [x] [greeter](https://github.com/micro/examples/tree/master/greeter) client/server {ruby, python, go}
  * [x] [go-micro/examples](https://github.com/micro/examples/tree/master)
  * [x] [pub sub](https://github.com/micro/examples/tree/master/pubsub)
  * [x] code generation example
  * [x] geo service and api
  * [x] slack bot API service
  * [x] wrappers/middleware

## Go Micro

* [x] Top level initialisation

### Middleware/Wrappers
  * [x] [Server](https://github.com/micro/go-micro/blob/master/server/server_wrapper.go)
  * [x] [Client](https://github.com/micro/go-micro/blob/master/client/client_wrapper.go)
  * [x] Example implementations
    * [x] [Client](https://github.com/micro/examples/tree/master/client/wrapper)
    * [x] [Server](https://github.com/micro/examples/blob/master/server/main.go#L12L28)
  * [x] Plugins e.g. trace, monitoring, logging

### Code generation
  * [x] Experimental generator [github.com/micro/protobuf](https://github.com/micro/protobuf)
  * [x] Example usage
    * [x] [Client](https://github.com/micro/examples/tree/master/client/codegen)
    * [x] [Server](https://github.com/micro/examples/tree/master/server/codegen)
  * [x] Server side generator
  * [x] Stable interface

### Registry
  * [x] Support Service TTLs on registration so services can be automatically removed
  * [x] Healthchecking function to renew registry lease
  * [x] Service/Node filters - known as a [Selector](https://github.com/micro/go-micro/blob/master/selector)
  * [x] Fix the watch code to return a channel with updates rather than store in memory
  * [x] Add timeout option for querying

### Broker
  * [x] Support distributed queuing
  * [x] Support acking of messages
  * [x] Support concurrency with options
  * [x] [MQTT](https://godoc.org/github.com/micro/go-micro/broker/mqtt)

### Transport
  * [x] Cleanup send/receive semantics - is it concurrent?

### Bidirectional streaming
  * [x] Client
  * [x] Server
  * [x] Code generation for streaming interface
  * [x] Examples

### TLS Support
  * [x] Registry
  * [x] Broker
  * [x] Transport

### Selector
  * [x] [Random](https://github.com/micro/go-micro/tree/master/selector/random)
  * [x] [Round Robin](https://github.com/micro/go-micro/tree/master/selector/roundrobin)
  * [x] [Blacklist](https://github.com/micro/go-micro/tree/master/selector/blacklist)

### Select Filters
  * [x] [Label](https://godoc.org/github.com/micro/go-micro/selector#FilterLabel)
  * [x] [Version](https://godoc.org/github.com/micro/go-micro/selector#FilterVersion)
  * [x] [Endpoint](https://godoc.org/github.com/micro/go-micro/selector#FilterEndpoint)

### Resiliency
  * [x] Add timeout and retry logic based on adrian cockcroft's ideas [here](http://www.slideshare.net/adriancockcroft/whats-missing-microservices-meetup-at-cisco)

### Debug
  * [x] Health
  * [x] Stats

## Go Plugins

  * [x] Provide more example implementations
  * [ ] Improve auto loading of plugins

### Registry
  * [x] [consul](https://godoc.org/github.com/micro/go-micro/registry/consul)
  * [x] [mdns](https://godoc.org/github.com/micro/go-micro/registry/mdns)
  * [x] [eureke](https://godoc.org/github.com/micro/go-plugins/registry/eureka)
  * [x] [etcd](https://godoc.org/github.com/micro/go-plugins/registry/etcd)
  * [x] [etcdv3](https://godoc.org/github.com/micro/go-plugins/registry/etcdv3)
  * [x] [nats](https://godoc.org/github.com/micro/go-plugins/registry/nats)
  * [x] [eureka](https://godoc.org/github.com/micro/go-plugins/registry/eureka)
  * [x] [gossip](https://godoc.org/github.com/micro/go-plugins/registry/gossip)
  * [x] [kubernetes](https://godoc.org/github.com/micro/go-plugins/registry/kubernetes)
  * [x] [zookeeper](https://godoc.org/github.com/micro/go-plugins/registry/zookeeper)

### Transport
  * [x] [http](https://github.com/micro/go-micro/tree/master/transport/http)
  * [x] [nats](https://godoc.org/github.com/micro/go-plugins/transport/nats)
  * [x] [tcp](https://godoc.org/github.com/micro/go-plugins/transport/tcp)
  * [x] [grpc](https://godoc.org/github.com/micro/go-plugins/transport/grpc)
  * [x] [rabbitmq](https://godoc.org/github.com/micro/go-plugins/transport/rabbitmq)

### Broker
  * [x] [nats](https://godoc.org/github.com/micro/go-plugins/broker/nats)
  * [x] [nsq](https://godoc.org/github.com/micro/go-plugins/broker/nsq)
  * [x] [mqtt](https://godoc.org/github.com/micro/go-plugins/broker/mqtt)
  * [x] [kafka](https://godoc.org/github.com/micro/go-plugins/broker/kafka)
  * [x] [rabbitmq](https://godoc.org/github.com/micro/go-plugins/broker/rabbitmq)
  * [x] [googlepubsub](https://godoc.org/github.com/micro/go-plugins/broker/googlepubsub)

### Client
  * [x] [grpc](https://github.com/micro/go-plugins/tree/master/client/grpc)

### Server
  * [x] [grpc](https://github.com/micro/go-plugins/tree/master/server/grpc)

## Deployments
* [x] [Kubernetes](https://github.com/micro/kubernetes)
* [x] [Docker Compose](https://github.com/micro/micro/blob/master/.compose.yml)


{% include links.html %}
