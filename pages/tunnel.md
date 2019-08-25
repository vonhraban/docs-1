---
title: Service Tunnel
keywords: tunnel
tags: [tunnel]
sidebar: home_sidebar
permalink: tunnel.html
summary: The micro tunnel is a point to point tunnel.
---

A service tunnel is a point to point tunnel used for accessing services in remote environments.

<img src="images/tunnel.svg" />

## Overview

The micro tunnel provides a way to access services across remote environments. This is great where you want to tunnel to staging, prod 
or expose local services externally without using something like openvpn or wireguard which would expose all things in your network.

## Install

```shell
go get -u github.com/micro/micro
```

## Dependencies

The tunnel uses Go Micro which means it depends on service discovery. The default is MDNS which means zeroconf. 

If you want something more resilient you can install consul and specify with the `--registry=consul` flag.

```
brew install consul
consul agent -dev
```

## Run Tunnel

Start the tunnel server (Runs on port :8083)

```shell
micro tunnel
```

## Tunnel Services

Now the tunnel is running you can connect to it with a local tunnel.

```
micro tunnel --server=remote.env:8083
```

Any request now made through the tunnel will be proxied to a service on the other side.

Set your proxy to use the tunnel
```
MICRO_PROXY=go.micro.tunnel go run main.go
```

Your service will direct all traffic through the tunnel. 

{% include links.html %}
