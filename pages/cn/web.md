---
title: Web管理控制台
keywords: web
tags: [web]
sidebar: home_sidebar_cn
permalink: /cn/web.html
lang: cn
summary: Web管理控制台提供控制功能、服务查看支持
---

## 安装

```bash
go get github.com/micro/micro
```

## 运行

```bash
micro web
```
使用浏览器打开`http://localhost:8082`

## 使用ACME协议

Micro Web支持`Let's Encrypt`开发的ACME协议，它可以帮你的域名颁发证书。

```
micro --enable_acme web
```

可以指定白名单：

```
micro --enable_acme --acme_hosts=example.com,api.example.com web
```

## 设置TLS证书

管理控制台也支持使用TLS证书

```bash
micro --enable_tls --tls_cert_file=/path/to/cert --tls_key_file=/path/to/key web
```

## 相关截屏

下面是Web运行起来访问的截屏：

<img src="../images/web1.png">

<img src="../images/web2.png">

<img src="../images/web3.png">

<img src="../images/web4.png">


{% include links.html %}
