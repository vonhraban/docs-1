---
title: Service Proxy
keywords: proxy
tags: [proxy]
sidebar: home_sidebar
permalink: proxy.html
summary: The micro proxy is a service-to-service proxy.
---

Service proxies offload distributed systems concerns to a piece of infrastructure rather than baking them into the client.

<img src="../images/proxy.svg" />

## Overview

The micro proxy provides a proxy implementation of the go-micro framework. This consolidates go-micro features into a single location which allows 
offloading service discovery, load balancing, fault tolerance, plugins, wrappers, etc to the proxy itself. Rather than updating every Go Micro 
app for infrastructure level concerns, its easier to put them in the proxy. It also allows any language to be integrated with a thin client 
rather than having to implement all the features.

## Getting Started

- [Install](#install)
- [Dependencies](#dependencies)
- [Run Proxy](#run)
- [ACME](#acme)
- [Proxy CLI](#proxy-cli)

## Install

```shell
go get -u github.com/micro/micro
```

## Dependencies

The proxy uses Go Micro which means it depends on service discovery. The default is MDNS which means zeroconf. 

If you want something more resilient you can install consul and specify with the `--registry=consul` flag.

```
brew install consul
consul agent -dev
```

## Run

The micro proxy http interface runs on port 8081 by default. 

Start the proxy

```shell
micro proxy
```

The go-micro server address is dynamic but can be configured as follows.

```
micro proxy
```

## Proxy Services

Now the proxy is running you can quite simply proxy requests through it.

Start your go micro app like so

```
MICRO_PROXY=go.micro.proxy go run main.go
```

Your service will lookup the proxy in discovery then use it to route any requests. If multiple proxies exist it will balance 
it's requests across them. It will also cache the proxy addresses locally.


If you would rather send requests through a single proxy specify it's address like so.

```
MICRO_PROXY_ADDRESS=localhost:9090 go run main.go
```

Ensure the proxy is running on the address specified.

```
MICRO_SERVER_ADDRESS=localhost:9090 micro proxy
```

{% include links.html %}
