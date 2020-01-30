---
title: 使用Kubernetes发布
keywords: kubernetes
tags: [kubernetes]
sidebar: home_sidebar_cn
lang: cn
permalink: /cn/deploy-kubernetes.html
summary: 
---

Micro在Kubernetes中是原生kubernetes的微服务。

Micro是一个微服务工具包，而Kubernetes是一个容器调度平台，它们组合在一起便构成微服务基础设施。

## 特性

- 无外部依赖
- 客户端缓存服务发现
- 可选k8s服务负载均衡
- 使用gRPC传输协议
- 预置工具包

## 安装 Micro

```
go get github.com/micro/kubernetes/cmd/micro
```

或者

```
docker pull micro/micro:kubernetes
```

如果使用go-micro，可以导入：

```
import "github.com/micro/kubernetes/go/micro"
```

## 编写服务

在[go-micro](https://github.com/micro/go-micro)中可以使用其它服务一样集成k8s。

```go
import (
	"github.com/micro/go-micro/v2"
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

## 部署服务

以下是k8s部署微服务的示例

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

## 健康检查器

健康检查器（healthchecking sidecar，参考sidecar模式）会调用服务rpc接口`Debug.Health`向外暴露`/health`端点，而每个go-micro服务都有一个内置的Debug.Health端点。

### 部署健康检查

```
go get github.com/micro/kubernetes/cmd/health
```

或者

```
docker pull microhq/health:kubernetes
```

### 运行健康检查

例如，运行healthcheck greeter服务，地址为localhost：9091

```
health --server_name=greeter --server_address=localhost:9091
```

在localhost:8080调用healthchecker 

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

## K8s负载均衡

Micro默认提供客户端负载均衡，但k8s也提供有服务负载均衡策略。

要把默认的均衡卸掉迁移到k8s，可以使用[静态选择器](https://github.com/micro/go-plugins/tree/master/selector/static)与k8s服务协同的方式。

不同于地址解析，选择器返回服务名与既定端口，例如greeter服务抬greeter:8080。

点击了解更多关于[静态选择器](https://github.com/micro/go-plugins/tree/master/selector/static)。

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

部署示例

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

静态选择器将负载均衡迁移到k8s服务。因此，请确保为每个微服务创建了k8s服务。

服务样例

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

通过kubectl部署

```
kubectl create -f service.yaml
```

这样，在你的服务中调用greeter微服务，会被路由到k8s服务中的greeter服务8080端口上。

{% include links.html %}
