---
title: Kubernetes Deployment
keywords: kubernetes
tags: [kubernetes]
sidebar: home_sidebar_cn
permalink: deploy-kubernetes_cn.html
summary: 
---

Micro 在 Kubernetes 中是原生 kubernetes 的微服务。

Micro是一个微服务工具包。 Kubernetes是一个容器调度平台。

它们共同为微服务平台提供了基础设施。

## 特性

- 无外部依赖
- 客户端发现缓存
- 可选的k8s服务负载均衡
- 使用gRPC传输协议
- 已初始化的工具包

## 快速入门

- [安装 Micro](#%E5%AE%89%E8%A3%85-micro)
- [编写服务](#%E7%BC%96%E5%86%99%E6%9C%8D%E5%8A%A1)
- [部署一个服务](#%E9%83%A8%E7%BD%B2%E4%B8%80%E4%B8%AA%E6%9C%8D%E5%8A%A1)
- [健康检查 Sidecar](#%E5%81%A5%E5%BA%B7%E6%A3%80%E6%9F%A5-sidecar)
- [K8s 负载负载均衡](#k8s-%E8%B4%9F%E8%BD%BD%E8%B4%9F%E8%BD%BD%E5%9D%87%E8%A1%A1)
- [Contribute](#contribute)

## 安装 Micro


```
go get github.com/micro/kubernetes/cmd/micro
```

或者

```
docker pull microhq/micro:kubernetes
```

对于 go-micro

```
import "github.com/micro/kubernetes/go/micro"
```

## 编写服务

像任何其他[go-micro](https://github.com/micro/go-micro)服务一样编写。


```go
import (
	"github.com/micro/go-micro"
	k8s "github.com/micro/kubernetes/go/micro"
)

func main() {
	service := k8s.NewService(
		micro.Name("greeter")
	)
	service.Init()
	service.Run()
}
```

## 部署一个服务

以下是k8s部署一个微服务的示例

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  namespace: default
  name: greeter
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: greeter-srv
    spec:
      containers:
        - name: greeter
          command: [
		"/greeter-srv",
		"--server_address=0.0.0.0:8080",
		"--broker_address=0.0.0.0:10001"
	  ]
          image: microhq/greeter-srv:kubernetes
          imagePullPolicy: Always
          ports:
          - containerPort: 8080
            name: greeter-port
```

通过 kubectl 部署

```
kubectl create -f greeter.yaml
```

## 健康检查 Sidecar

healthchecking sidecar将`/ health`暴露为http端点，并在服务上调用rpc端点`Debug.Health`。
每个go-micro服务都有一个内置的Debug.Health端点。

### 部署健康检查

```
go get github.com/micro/health
```

或者

```
docker pull microhq/health:kubernetes
```

### 运行健康检查

运行例如healthcheck greeter service，地址为localhost：9091

```
health --server_name=greeter --server_address=localhost:9091
```

在 localhost:8080 调用 healthchecker 

```
curl http://localhost:8080/health
```

### K8s 部署

添加 kubernetes 部署配置

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  namespace: default
  name: greeter
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: greeter-srv
    spec:
      containers:
        - name: greeter
          command: [
		"/greeter-srv",
		"--server_address=0.0.0.0:8080",
		"--broker_address=0.0.0.0:10001"
	  ]
          image: microhq/greeter-srv:kubernetes
          imagePullPolicy: Always
          ports:
          - containerPort: 8080
            name: greeter-port
        - name: health
          command: [
		"/health",
                "--health_address=0.0.0.0:8081",
		"--server_name=greeter",
		"--server_address=0.0.0.0:8080"
	  ]
          image: microhq/health:kubernetes
          livenessProbe:
            httpGet:
              path: /health
              port: 8081
            initialDelaySeconds: 3
            periodSeconds: 3
```

## K8s 负载负载均衡
Micro 默认包括客户端负载均衡，但 kubernetes 还提供服务的负载平衡策略。我们可以使用[静态选择器](https://github.com/micro/go-plugins/tree/master/selector/static)和 k8s 服务将负载均衡迁移到k8s中。

静态选择器不是进行地址解析，而是返回服务名称和固定端口，例如greeter返回greeter:8080。阅读关于[静态选择器](https://github.com/micro/go-plugins/tree/master/selector/static)的内容。

### 用法

要在运行服务时使用静态选择器，请指定参数或环境变量

```
MICRO_SELECTOR=static ./service
```

或者

```
./service --selector=static
```

### K8s 部署

一个部署示例

```
apiVersion: extensions/v1beta1
kind: Deployment
metadata:
  namespace: default
  name: greeter
spec:
  replicas: 1
  template:
    metadata:
      labels:
        app: greeter-srv
    spec:
      containers:
        - name: greeter
          command: [
		"/greeter-srv",
		"--selector=static",
		"--server_address=0.0.0.0:8080",
		"--broker_address=0.0.0.0:10001"
	  ]
          image: microhq/greeter-srv:kubernetes
          imagePullPolicy: Always
          ports:
          - containerPort: 8080
            name: greeter-port
```

### K8s 服务

静态选择器将负载均衡迁移到k8s服务。 因此，请确保为每个微服务创建了k8s服务。

这是一个服务样例

```
apiVersion: v1
kind: Service
metadata:
  name: greeter
  labels:
    app: greeter
spec:
  ports:
  - port: 8080
    protocol: TCP
  selector:
    app: greeter
```

通过 kubectl 部署

```
kubectl create -f service.yaml
```

通过其他服务调用微服务 "greeter" 将会指向 k8s 服务 greeter:8080。


{% include links.html %}
