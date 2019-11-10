---
title: 调试信息
keywords: debug
tags: [debug]
sidebar: home_sidebar_cn
lang: cn
permalink: /cn/enable-debug.html
summary: 
---

Go-micro支持设置日志打印级别以及采集运行概要。

## 打印日志

调整日志级别

```go
MICRO_LOG_LEVEL=debug go run main.go
```

目前日志支持的级别：

```
trace
debug
error
info
```

## Profiling 运行概要

支持激活Go pprof采集运行概要

```go
MICRO_DEBUG_PROFILE=true go run main.go
```

该变量会激活cpu及内存heap运行的信息到 **/tmp/[service name].[service version].{cpu, mem}.pprof**中。