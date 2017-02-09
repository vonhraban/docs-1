---
title: Micro Docs
keywords: kubernetes
tags: [kubernetes]
sidebar: home_sidebar
permalink: deploy-kubernetes.html
summary: 
---

# Kubernetes Deployment

Micro easily runs on kubernetes an can even use the kubernetes API for service discovery.

## Configuration

The repo [github.com/micro/kubernetes](https://github.com/micro/kubernetes) provides an example configuration for Micro on Kubernetes

```
git clone https://github.com/micro/kubernetes
```

### What's in the repo?

Currently config to run Micro on Kubernetes (Testing on Google Container Engine).

- Micro - API, Web UI and Sidecar (spins up GCE Load Balancers)
- Services - Some example micro services
- run.sh - a simple shell script which uses kubectl to start/stop services

## Getting Started

Here's the steps I took to get started.

### Run Kubernetes

GKE is the easiest way to run a managed kubernetes cluster. What's even better? $300 free credit for 60 days.

1. Get yourself a [Free Trial](https://cloud.google.com/free-trial/) of Google Container Engine
2. Visit the [Quickstart](https://cloud.google.com/container-engine/docs/quickstart) guide to create a cluster

### Run Micro

Make sure kubectl is in your path

```shell
./run.sh start
```

{% include links.html %}
