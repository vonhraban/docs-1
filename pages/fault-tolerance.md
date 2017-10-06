---
title: Fault Tolerance
keywords: fault tolerance
tags: [fault tolerance]
sidebar: home_sidebar
permalink: fault-tolerance.html
summary: 
---

# Fault Tolerance

In a distributed systems world failures are occurring all the time. Micro attempts to address this with some best practices for fault tolerance. 
This document describes some of the ways in which they can be configured.

## Heartbeating

Heartbeating is a mechanism of refreshing registration in service discovery.

### Rationale

Services register with service discovery on startup and deregister on shutdown. Sometimes these services may unexpectedly die, 
be killed forcefully or face transient network issues. In these cases stale nodes will be left in service discovery. It would be 
ideal if services were automatically removed.

### Solution

Micro supports the option of a register TTL and register interval for this exact reason. TTL specifies how long a registration should 
exist in discovery after which it expires and is removed. Interval is the time at which a service should re-register to preserve 
it's registration in service discovery.

These are options made available in go-micro and as flags in the micro toolkit

### Usage

For the micro toolkit simply use the built in flags to set ttl and interval.

```
micro --register_ttl=30 --register_interval=15 api
```

This example shows that we're setting a ttl of 30 seconds with a re-register interval of 15 seconds.

For go-micro, when declaring a micro service you can pass in the options as time.Duration

```
service := micro.NewService(
        micro.Name("com.example.srv.foo"),
        micro.RegisterTTL(time.Second*30),
        micro.RegisterInterval(time.Second*15),
)
```

## Caching Discovery

A discovery cache is a client side cache of service discovery information

### Rationale

Service discovery is the core dependency of microservices but can also become a single point of failure if not architected correctly. Every 
discovery system has its own properties for scaling and high availability. In the event discovery goes down it makes the rest of the system 
unusable as services do not know how to resolve names to addresses. Discovery can also be a bottleneck if a lookup is done for every 
request in the system.

### Solution

Client side caching is a method of eliminating service discovery as a bottleneck and single point of failure. Micro includes a selector 
(client side load balancer) which maintains an in memory cache of service discovery information relevant to it. If a cache miss occurs 
the selector will use the service registry to do a lookup and cache the data. The cache is also periodically TTLed out to ensure 
stale data does not live on.

### Usage

The caching selector can be set by flag or when creating a new service.

As a flag with the toolkit do the following

```
micro --selector=cache api
```

Go-micro services can also use the same flag if the Init method is called. Alternatively the selector can be set in code.

```
import (
	"github.com/micro/go-micro/client"
	"github.com/micro/go-micro/selector/cache"
)

service := micro.NewService(
	micro.Name("com.example.srv.foo"),
)

service.Client().Init(cache.NewSelector())
```

{% include links.html %}
