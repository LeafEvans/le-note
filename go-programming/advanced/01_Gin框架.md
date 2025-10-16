# Gin 框架

## 介绍

Gin 是一个用 Go（Golang）编写的轻量级 HTTP Web 框架，以其出色的性能和高效著称。如果你追求极致的运行速度和开发效率，Gin 是一个理想的选择。

Gin 尤其擅长处理高并发的 API 接口。对于规模不大、业务逻辑相对简单的项目，Gin 同样非常适用。

当某个接口面临较大的性能瓶颈时，也可以考虑使用 Gin 对其进行重构，以提升响应速度和吞吐能力。

## 环境搭建

下载并安装 Gin：

```sh
go get -u github.com/gin-gonic/gin
```

将 Gin 引入到代码中：

```go
import "github.com/gin-gonic/gin"
```

（可选）如果使用 `http.StatusOK` 之类的常量，则需要引入 `net/http` 包：

```go
import "net/http"
```

新建 `main.go` 配置路由：

```go
package main

import (
	"net/http"

	"github.com/gin-gonic/gin"
)

func main() {
	// 创建一个默认路由引擎
	r := gin.Default()
	// 配置路由
	r.GET("/", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"message": "Hello World!",
		})
	})
	// 启动 HTTP 服务，默认在 0.0.0.0:8080 启动服务
	r.Run()
}

```

<img src="../../images/image-202510161828.webp" style="zoom:80%;" />

要改变默认启动的端口：

```go
r.Run(":9000")
```

## 热加载

**热加载（Hot Reload）** 是指在开发过程中，当代码发生修改时，程序能够自动重新编译并重启，无需手动干预。这一功能极大提升了开发效率，使我们能够即时验证代码变更，省去了反复手动编译和重启的繁琐步骤。 

在 Beego 框架中，官方提供了 `bee` 工具，原生支持热加载，开箱即用。

而 Gin 作为轻量级框架，并未内置热加载功能，但我们可以借助优秀的第三方工具（如 `air`、`fresh` 或 `gowatch`）来实现相同的效果。 

以 `air` 为例：

`air` 是一个专为 Go 项目设计的轻量级热重载工具，支持自动监听文件变化、重新编译并重启服务，且配置灵活、启动迅速，已成为 Gin 等轻量框架开发中的热门选择。 

**通过 Go 安装（需 Go 1.17+）**：

```sh
go install github.com/air-verse/air@latest
```

**（可选）初始化配置，在项目根目录下运行**：

```sh
air init
```

会生成 `.air.toml` 配置文件，可以根据需要自定义监听目录、忽略文件、构建命令。

**启动热加载，在项目根目录运行**：

```sh
air
```

此时 `air` 会自动编译并运行主程序（默认为 `main.go`），并在修改 `.go` 文件时自动启动重启服务。

## 路由

### 概述

**路由（Routing）**是由一个 URL（或者称为路径）以及一个特定的 HTTP 方法（`GET`、`POST` 等）组成的，涉及到应用如何响应客户端对某个网站节点的访问。

RESTful API 是目前比较成熟的一套互联网应用程序 API 设计理念，因此自行设计时参考 RESTful API 指南。

在 RESTful 架构中，每个网址代表一种资源，不同的请求方式表示执行不同的操作：

| 方法                      | 说明                                           |
| ------------------------- | ---------------------------------------------- |
| `GET`（Retrieve）         | 获取服务器上的一个或多个资源                   |
| `POST`（Create）          | 在服务器上创建一个新资源                       |
| `PUT`（Update）           | 在服务器上更新资源（需提供完整的新资源表示）   |
| `PATCH`（Partial Update） | 对资源进行部分更新（仅提交需修改的字段）       |
| `DELETE`（Delete）        | 从服务器上删除指定资源                         |
| `HEAD`（Metadata）        | 获取资源的元信息（如响应头），不返回响应体     |
| `OPTIONS`（Discovery）    | 查询服务器对某资源支持的 HTTP 方法或 CORS 策略 |

### 简单路由配置

使用 `GET` 请求访问一个网址时：

```go
	r.GET("/select", func(c *gin.Context) {
		c.String(http.StatusOK, "Get")
	})
```

<img src="../../images/image-202510161911.webp" style="zoom:80%;" />

使用 `POST` 访问一个网址时：

```go
	r.POST("/create", func(c *gin.Context) {
		c.String(http.StatusOK, "Post")
	})
```

<img src="../../images/image-202510161939.webp" style="zoom:80%;" />

使用 `DELETE` 访问一个网址时：

```go
	r.DELETE("/delete", func(c *gin.Context) {
		c.String(http.StatusOK, "Delete")
	})
```

<img src="../../images/image-202510161941.webp" style="zoom:80%;" />

在路由中获取 `GET` 传值，域名为 `/news?aid=20`：

```go
	r.GET("/news", func(c *gin.Context) {
		aid := c.Query("aid")
		c.String(200, "aid = %s", aid)
	})
```

<img src="../../images/image-202510161944.webp" style="zoom:80%;" />

动态路由，域名为 `/user/20`：

```go
	r.GET("/user/:uid", func(c *gin.Context) {
		uid := c.Param("uid")
		c.String(200, "userID = %s", uid)
	})
```

<img src="../../images/image-202510161947.webp" style="zoom:80%;" />

### 响应数据格式

#### `c.String()`

返回一个字符串。

```go
	r.GET("/", func(c *gin.Context) {
		c.String(http.StatusOK, "值：%v", "首页")
	})
```

<img src="../../images/image-202510161952.webp" style="zoom:80%;" />

#### `c.JSON()`

返回一个 JSON 数据。

```go
	r.GET("/json", func(c *gin.Context) {
		c.JSON(http.StatusOK, map[string]any{
			"success": true,
			"message": "Hello Gin!",
		})
	})
```

或者：

```go
	r.GET("/json", func(c *gin.Context) {
		c.JSON(http.StatusOK, gin.H{
			"success": true,
			"message": "Hello Gin!",
		})
	})
```

<img src="../../images/image-202510161959.webp" style="zoom:80%;" />

因为 `c.JSON` 函数的第二个参数类型为 `any`，因此也可以自定义结构体返回：

```go
	r.GET("/struct", func(c *gin.Context) {
		a := &Article{
			"title", "desc", "content",
		}
		c.JSON(http.StatusOK, a)
	})
```

<img src="../../images/image-202510162006.webp" style="zoom:80%;" />

需要小写字段则添加对应的 Tag：

```go
type Article struct {
	Title   string `json:"title"`
	Desc    string `json:"desc"`
	Content string `json:"content"`
}
```

<img src="../../images/image-202510162010.webp" style="zoom:80%;" />

#### `c.JSONP` 

```go
	r.GET("/jsonp", func(c *gin.Context) {
		a := &Article{
			"title-jsonp", "desc", "content",
		}
		c.JSONP(http.StatusOK, a)
	})
```

使用 JSONP 格式时，需通过 `callback` 查询参数指定回调函数名（例如 `/jsonp?callback=xxxx`），此时响应将被包裹在名为 `xxxx` 的函数调用中。

<img src="../../images/image-202510162016.webp" style="zoom:80%;" />

#### `c.XML`

与 JSON 格式类似，有：

```go
	r.GET("/xml", func(c *gin.Context) {
		c.XML(http.StatusOK, gin.H{
			"success": true,
			"message": "Hello XML!",
		})
	})
```

<img src="../../images/image-202510162024.webp" style="zoom:80%;" />

也可以自定结构体进行返回：

```go
	r.GET("/struct-xml", func(c *gin.Context) {
		a := &Article{
			"title", "desc", "content",
		}
		c.XML(http.StatusOK, a)
	})
```

<img src="../../images/image-202510162026.webp" style="zoom:80%;" />

#### `c.HTML()`

```go
	r := gin.Default()
	// 加载模板
	r.LoadHTMLGlob("templates/*")
	r.GET("/news", func(c *gin.Context) {
		c.HTML(http.StatusOK, "news.html", gin.H{
			"title": "我是后台数据",
		})
	})
```

<img src="../../images/image-202510162044.webp" style="zoom:80%;" />

在 HTML 模板中，使用 `{{}}` 包裹变量以访问后台传递的数据，例如下面的 `title`：

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Document</title>
  </head>
  <body>
    <h2>{{.title}}</h2>
  </body>
</html>
```

<img src="../../images/image-202510162053.webp" style="zoom:80%;" />

