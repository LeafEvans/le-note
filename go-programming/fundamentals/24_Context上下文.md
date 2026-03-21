# Context 上下文

## 为什么需要 Context？

在 Go 语言的 Web 服务（如 `net/http` 包）中，每一个请求都会分配一个对应的 Goroutine 去处理。在处理请求的过程中，往往还需要启动额外的资 Goroutine 去访问数据库、RFC 服务等。

这些 Goroutine 之间通常需要共享**请求作用域的数据**（如身份认证 Token、TraceID）、