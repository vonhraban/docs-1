---
title: Local Deployment
keywords: local
tags: [local]
sidebar: home_sidebar
permalink: deploy-local.html
summary: 
---

Micro is incredibly simple to spin up locally

## Install

From source

```
go get github.com/micro/micro
```

Release binary

```
curl -fsSL https://micro.mu/install.sh | /bin/bash
```

## Run

Running micro is as simple as typing `micro`.

```
micro
```

## Verify

Check everythings working by using a few commands

```
# list local services

micro list services

# list network nodes

micro network nodes

# call a service

micro call go.micro.network Debug.Health
```

{% include links.html %}
