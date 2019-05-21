---
title: Go API 处理器
keywords: go-api
tags: [go-api]
lang: cn
sidebar: home_sidebar_cn
permalink: "/cn/go-api-handlers.html"
summary: 
---

API的Handler处理器都是用来接收Http请求，然后根据请求类型进行处理，或向前转发，或触发事件，为了方便，handler使用服务方法`http.Handler`的名字来路由。
 
  
## API Handlers

处理http请求，通过RPC对控制http请求与响应，API是默认的处理器，它接收任何HTTP请求并转成指定格式的RPC请求。

- Content-Type: Any
- Body: Any
- Forward Format: [api.Request](https://github.com/micro/go-api/blob/master/proto/api.proto#L11)/[api.Response](https://github.com/micro/go-api/blob/master/proto/api.proto#L21)
- Path: `/[service]/[method]`
- Resolver: 请求解析器，路径会被解析成服务与方法

## Broker Handler

broker handler订阅分发处理器，以http的方式实现go-micro的分发接口服务

- Content-Type: Any
- Body: Any
- Forward Format: HTTP
- Path: `/`
- Resolver: 订阅主题通过query请求参数指定

## CloudEvents Handler

云事件处理器提供HTTP入口，并把收到的请求转成CloudEvents消息通过消息总线的`go-micro/client.Publish`方法传出去。

- Content-Type: Any
- Body: Any
- Forward Format: 请求会按照[CloudEvents](https://github.com/cloudevents/spec)格式化
- Path: `/[topic]`
- Resolver: 请求路径会用来解析主题

## Event Handler

事件处理器提供HTTP入口，把请求转成消息调用`go-micro/client.Publish`方法通过消息总线发送出去。

- Content-Type: Any
- Body: Any
- Forward Format: 请求会按照[go-api/proto.Event](https://github.com/micro/go-api/blob/master/proto/api.proto#L28L39)格式化
- Path: `/[topic]/[event]`（可以看到与CloudEvents的不同）
- Resolver: 请求路径会用来解析主题及事件名

## HTTP Handler

Http处理器是HTTP的反向代理，内置在服务发现中。

- Content-Type: Any
- Body: Any
- Forward Format: 反向代理
- Path: `/[service]`
- Resolver: 请求路径会用来解析服务名

## Registry Handler

注册处理器接收HTTP请求，它负责go-micro的注册接口请求

- Content-Type: Any
- Body: JSON
- Forward Format: HTTP
- Path: `/`
- Resolver: 获取服务，注册与卸载服务分别通过GET, POST, DELETE请求处理。

## RPC Handler

RPC处理器接收JSON或protobuf的HTTP请求，并转成RPC请求向前转发。

- Content-Type: `application/json` or `application/protobuf`
- Body: JSON 或者 Protobuf
- Forward Format: json-rpc 或 proto-rpc，根据请求内容决定
- Path: `/[service]/[method]`
- Resolver: 请求路径会用来解析服务名与方法名

## Web Handler

web处理器职责是反射代理，它内置在服务发现中，并支持web socket。

- Content-Type: Any
- Body: Any
- Forward Format: 反向代理且支持web socket
- Path: `/[service]`
- Resolver: 请求路径会用来解析服务名


{% include links.html %}
