---
title: Docs
keywords: homepage
tags: [introduction]
sidebar: home_sidebar
permalink: index.html
---

Micro addresses the key requirements for building microservice systems. It takes the microservice architecture pattern and transforms 
it into a set of tools which act as the building blocks for scalable platforms. Micro hides the complexity of distributed systems and 
provides well understood concepts to developers.

## Overview

The main piece of software provided is [Micro](https://github.com/micro/micro), a microservice development runtime.

<center>
<img src="https://micro.mu/runtime3.svg" style="width: 100%; max-width: 600px; height: auto; margin-bottom: 25px;" />
</center>

The runtime is composed of the following features:

- **API Gateway:** A single entry point with dynamic request routing using service discovery. The API gateway allows you to build a scalable 
microservice architecture on the backend and consolidate serving a public api on the frontend. The micro api provides powerful routing 
via discovery and pluggable handlers to serve http, grpc, websockets, publish events and more.

- **Go Framework:** Leverage the powerful [Go Micro](https://github.com/micro/go-micro) framework to develop microservices easily and quickly. 
Go Micro abstracts away the complexity of distributed systems and provides simpler abstractions to build highly scalable microservices.

- **Interactive CLI:** A CLI to describe, query and interact directly with your platform and services from the terminal. The CLI 
gives you all the commands you expect to understand what's happening with your micro services. It also includes an interactive mode.

- **Multi-Cloud Network:** Build cross datacenter networks with the micro network. Simply drop-in and connect the network services across any environment 
and create a single flat network to route globally. The micro network dynamically builds routes based on your local registry in each datacenter 
ensuring queries are routed based on locality.

- **Service Proxy:** A transparent proxy built on [Go Micro](https://github.com/micro/go-micro). Offload service discovery, load balancing, 
fault tolerance, message encoding, middleware, monitoring and more to a single a location. Run it standalone or alongside your service.

- **Service Tunnel:** Get access to services in any environment without the need for a vpn. The micro tunnel provides point to 
point tunnelling with a built-in proxy to query services in remote environments. Query production systems from your local laptop. 

- **Slack Bot:** A bot which runs on your platform and lets you manage your applications from Slack itself. The micro bot enables ChatOps 
and gives you the ability to do everything with your team via messaging. It also includes ability to create slack commands as services which 
are discovered dynamically.

- **Template Generator:** Create new service templates to get started quickly. Micro provides predefined templates for writing micro services. 
Always start in the same way, build identical services to be more productive.

- **Web Dashboard:** The web dashboard allows you to explore your services, describe their endpoints, the request and response formats and even 
query them directly. The dashboard also includes a built in CLI like experience for developers who want to drop into the terminal on the fly.

## Getting Started

- Start writing services using the [go-micro](go-micro.html) framework.
- Interact with your service using the [micro](runtime.html) runtime.

## Resources

- Look at [examples](https://github.com/micro/examples) of how to use micro on github.
- Search the [explorer](https://micro.mu/explore/) to find existing open source projects on github.
- Read the [blog](https://micro.mu/blog/) for a deeper look at micro and broader microservice ideas.
- Watch the [video](https://www.youtube.com/watch?v=xspaDovwk34) on Simplifying Microservices with Micro presented at Golang UK Conf 2016.
- View the [slides](https://speakerdeck.com/asim) for various presentations on speakerdeck.

{% include links.html %}
