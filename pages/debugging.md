---
title: Debugging
keywords: debug
tags: [debug]
sidebar: home_sidebar
permalink: debugging.html
summary: 
---

Enable debugging of micro or go-micro very simply via the following environment variables.

## Logging

To enable debug logging

```
MICRO_LOG_LEVEL=debug
```

The log levels supported are

```
trace
debug
error
info
```

## Profiling

To enable profiling via pprof

```
MICRO_DEBUG_PROFILE=true
```

This will write a cpu and heap profile to /tmp/[service name].[service version].{cpu, mem}.pprof

{% include links.html %}
