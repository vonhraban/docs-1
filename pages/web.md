---
title: Web
keywords: web
tags: [web]
sidebar: home_sidebar
permalink: web.html
summary: Micro Web provides a dashboard to visualise and explore services
---

## Install

```bash
go get github.com/micro/micro
```

## Run

```bash
micro web
```
Browse to localhost:8082

## Use ACME

The micro web dashboard supports ACME via Let's Encrypt. It automatically gets a TLS certificate for your domain.

```
micro --enable_acme web
```

Optionally specify a host whitelist

```
micro --enable_acme --acme_hosts=example.com,api.example.com web
```

## Set TLS Certificate

The dashboard supports serving securely with TLS certificates

```bash
micro --enable_tls --tls_cert_file=/path/to/cert --tls_key_file=/path/to/key web
```

## Screenshots

<img src="images/web1.png">

<img src="images/web2.png">

<img src="images/web3.png">

<img src="images/web4.png">


{% include links.html %}
