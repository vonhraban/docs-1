---
title: Go API
keywords: go-api
tags: [go-micro]
lang: cn
sidebar: home_sidebar_cn
permalink: "/cn/go-api.html"
summary: 
---

Go API是基于服务发现驱动开发的可插拔API框架，旨在帮助构建强大的API网关。

## 概览

Go API支持网关路由。微服务架构是把应用解耦成逻辑上的相对隔离的服务，API网关提供单一的入口把服务的API统一起来。Go API使用服务发现中定义的元数据来生成路由规则并提供与之对应的http服务。

<img src="https://micro.mu/docs/images/go-api.png?v=1" alt="Go API" />

运行时工具[micro api](https://micro.mu/docs/cn/api.html)便是基于Go API构建。

## Handler处理器

Handler专门用于接收并处理HTTP请求，为了方便，handler使用服务方法`http.Handler`的名字来路由。

**micro api**支持以下类型的网关转发

- [`api`](#api-handler) - http转RPC请求
- [`broker`](#broker-handler) - pub/sub 异步请求
- [`cloudevents`](#cloudevents-handler) - 专门处理[CloudEvent](https://github.com/cloudevents/spec)事件，并向消息总线分发消息
- [`event`](#event-handler) - 处理任意的http请求并向消息总线分发消息。
- [`http`](#http-handler) - 处理http请求并转向反向代理。
- [`registry`](#registry-handler) - http 注册
- [`rpc`](#rpc-handler) - 处理json及protobuf格式的POST请求，并转向RPC。
- [`web`](#web-handler) - 包含web socket的http反向代理。

## API Handler

处理http请求，通过RPC对控制http请求与响应，API是默认的处理器，它接收任何HTTP请求并转成指定格式的RPC请求。

- Content-Type: 所有类型
- Body: 任务类型
- Forward Format: [api.Request](https://github.com/micro/go-micro/blob/master/api/proto/api.proto#L11)/[api.Response](https://github.com/micro/go-micro/blob/master/api/proto/api.proto#L21)
- Path: `/[service]/[method]`
- Resolver: 请求解析器，路径会被解析成服务与方法

## Broker Handler

broker handler Pub/Sub 订阅分发处理器，以http的方式实现go-micro的分发接口服务

- Content-Type: Any
- Body: Any
- Forward Format: HTTP
- Path: `/`
- Resolver: 订阅主题通过query请求参数指定

## CloudEvents Handler

云事件处理器提供HTTP入口，并把收到的请求转成[CloudEvent](https://github.com/cloudevents/spec)消息通过消息总线的`go-micro/client.Publish`方法传出去。

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

web处理器职责是反向代理，它内置服务发现，并支持web socket。

- Content-Type: Any
- Body: Any
- Forward Format: 反向代理且支持web socket
- Path: `/[service]`
- Resolver: 请求路径会用来解析服务名

{% include links.html %}
