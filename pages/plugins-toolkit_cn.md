---
title: 工具集插件
keywords: plugins
tags: [plugins]
sidebar: home_sidebar_cn
lang: cn
permalink: plugins-toolkit_cn.html
summary: 
---

# 工具集插件

外部代码可以使用插件的方式集成到Micro工具集中。需要注意的是，这与go-micro的工具集是完全分离的。

当使用插件后，就可以在工具集中增加flag、命令行、http处理器。

## 插件是如何工作的

micro/plugin包含了插件，其下有一个全局插件管理器可以跨全工具集使用。

插件通过调用`plugin.Register`方法注册。每个组件（api, web, proxy, cli, bot）都各有一个用于注册插件的管理器，该管理器只能作为其所属组件的一部分加到组件中。可以通过调用`api.Register`、`web.Register`等等使用它们。

下面是插件接口

```go
// Plugin is the interface for plugins to micro. It differs from go-micro in that it's for
// the micro API, Web, Proxy, CLI. It's a method of building middleware for the HTTP side.
type Plugin interface {
	// Global Flags
	Flags() []cli.Flag
	// Sub-commands
	Commands() []cli.Command
	// Handle is the middleware handler for HTTP requests. We pass in
	// the existing handler so it can be wrapped to create a call chain.
	Handler() Handler
	// Init called when command line args are parsed.
	// The initialised cli.Context is passed in.
	Init(*cli.Context) error
	// Name of the plugin
	String() string
}

// Manager is the plugin manager which stores plugins and allows them to be retrieved.
// This is used by all the components of micro.
type Manager interface {
        Plugins() map[string]Plugin
        Register(name string, plugin Plugin) error
}

// Handler is the plugin middleware handler which wraps an existing http.Handler passed in.
// Its the responsibility of the Handler to call the next http.Handler in the chain.
type Handler func(http.Handler) http.Handler
```

## 如何使用

下面我们展示一个简单的插件，它负责接收flag参数，并打印该参数值。

### 插件

Create a plugin.go file in the top level dir

在顶级目录下增加plugin.go文件

```go
package main

import (
	"log"
	"github.com/micro/cli"
	"github.com/micro/micro/plugin"
)

func init() {
	plugin.Register(plugin.NewPlugin(
		plugin.WithName("example"),
		plugin.WithFlag(cli.StringFlag{
			Name:   "example_flag",
			Usage:  "This is an example plugin flag",
			EnvVar: "EXAMPLE_FLAG",
			Value: "avalue",
		}),
		plugin.WithInit(func(ctx *cli.Context) error {
			log.Println("Got value for example_flag", ctx.String("example_flag"))
			return nil
		}),
	))
}
```

### 编译文件

指定plugin文件编译micro应用

```shell
go build -o micro ./main.go ./plugin.go
```

## 仓库

micro工具集可以在[github.com/micro/go-plugins/micro](https://github.com/micro/go-plugins/tree/master/micro)查看。

{% include links.html %}
