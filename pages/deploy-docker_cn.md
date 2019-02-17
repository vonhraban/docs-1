---
title: 使用Docker发布
keywords: docker
tags: [docker]
sidebar: home_sidebar_cn
lang: cn
permalink: deploy-docker_cn.html
summary: 
---

# 使用 Docker 部署

Micro很容易在docker容器内运行

## 预置镜像

我们在Docker镜像站[Docker Hub](https://hub.docker.com/r/microhq/) 上提供了预置镜像

### 获取micro

```
docker pull microhq/micro
```

## Compose配置

在本地通过compose.yml运行

```
consul:
  command: -server -bootstrap -rejoin 
  image: progrium/consul:latest
  hostname: "registry"
  ports:
  - "8300:8300"
  - "8400:8400"
  - "8500:8500"
  - "8600:53/udp"
api:
  command: --registry_address=registry:8500 --register_interval=5 --register_ttl=10 api
  build: .
  links:
  - consul
  ports:
  - "8080:8080"
proxy:
  command: --registry_address=registry:8500 --register_interval=5 --register_ttl=10 proxy
  build: .
  links:
  - consul
  ports:
  - "8081:8081"
web:
  command: --registry_address=registry:8500 --register_interval=5 --register_ttl=10 web
  build: .
  links:
  - consul
  ports:
  - "8082:8082"
```

## 从原始镜像开始构建

在[micro](https://github.com/micro/micro/blob/master/Dockerfile)仓库中我们放有Dockerfile文件：

```
FROM alpine:3.2
RUN apk add --update ca-certificates && \
    rm -rf /var/cache/apk/* /tmp/*
ADD micro /micro
WORKDIR /
ENTRYPOINT [ "/micro" ]
```

### 构建二进制文件

```
CGO_ENABLED=0 GOOS=linux go build -a -installsuffix cgo -ldflags '-w' -i -o micro ./main.go 
```

### 构建镜像

```
docker build -t micro .
```

{% include links.html %}
