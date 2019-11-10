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

## Features

The runtime is composed of the following features:

- **api**: An api gateway. A single entry point with dynamic request routing using service discovery. The API gateway allows you to build a scalable 
microservice architecture on the backend and consolidate serving a public api on the frontend. The micro api provides powerful routing 
via discovery and pluggable handlers to serve http, grpc, websockets, publish events and more.

- **bot:** A slackbot which runs on your platform and lets you manage your applications from Slack itself. The micro bot enables ChatOps 
and gives you the ability to do everything with your team via messaging. It also includes ability to create slack commands as services which 
are discovered dynamically.

- **cli:** An interactive CLI to describe, query and interact directly with your platform and services from the terminal. The CLI 
gives you all the commands you expect to understand what's happening with your micro services. It also includes an interactive mode.

- **network:** Build multi-cloud networks with the micro network service. Simply drop-in and connect the network services across any environment 
and create a single flat network to route globally. The micro network dynamically builds routes based on your local registry in each datacenter 
ensuring queries are routed based on locality.

- **new:** A service template generator. Create new service templates to get started quickly. Micro provides predefined templates for writing micro services. 
Always start in the same way, build identical services to be more productive.

- **proxy:** A transparent service proxy built on [Go Micro](https://github.com/micro/go-micro). Offload service discovery, load balancing, 
fault tolerance, message encoding, middleware, monitoring and more to a single a location. Run it standalone or alongside your service.

- **tunnel:** A network tunnel to get access to services in any environment without the need for a vpn. The micro tunnel provides point to 
point tunnelling with a built-in proxy to query services in remote environments. Query production systems from your local laptop. 

- **web:** The web dashboard allows you to explore your services, describe their endpoints, the request and response formats and even 
query them directly. The dashboard also includes a built in CLI like experience for developers who want to drop into the terminal on the fly.

Additionally micro provides a Go development framework:

- **go-micro:** Leverage the powerful [Go Micro](https://github.com/micro/go-micro) framework to develop microservices easily and quickly. 
Go Micro abstracts away the complexity of distributed systems and provides simpler abstractions to build highly scalable microservices.# Getting Started

## Getting Started

- **Framework** - Start writing services using the [go-micro](framework.html) framework.
- **Runtime** - Manage your services using the [micro](runtime.html) runtime.
- **Network** - Publicly share and host your services on the [network](network.html).
- **Examples** - Learn by example with real code [examples](https://github.com/micro/examples)
- **Community** - Join the the community and start collaborating on [slack](https://micro.mu/slack)

{% include links.html %}
