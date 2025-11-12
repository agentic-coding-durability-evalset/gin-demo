# Gin Demo

一个基于 [Gin](https://gin-gonic.com/) 框架的 Go Web 应用示例项目。Gin 是一个用 Go 编写的 HTTP Web 框架，具有类似 Martini 的 API，但性能更好。

## 技术栈

- **Go**: 1.25.3
- **Gin**: 1.11.0

## 项目结构

```
gin-demo/
├── main.go              # 应用入口，路由和处理器定义
├── go.mod               # Go 模块依赖
├── go.sum               # 依赖校验和
├── index.http           # HTTP 请求测试文件
└── README.md
```

## 功能特性

- 基础路由处理
- JSON 响应
- 中间件支持（日志和恢复）
- 简洁的 API 设计

## 快速开始

### 前置要求

- Go 1.25.3 或更高版本

### 安装和运行

```bash
# 安装依赖
go mod download

# 运行项目
go run main.go
```

服务将在 `http://localhost:8080` 启动（Gin 默认端口）。

### 构建可执行文件

```bash
# 构建
go build -o gin-demo

# 运行
./gin-demo
```

## API 端点

### Ping 端点
```http
GET http://localhost:8080/ping
```
响应示例:
```json
{
  "message": "pong"
}
```

## 代码说明

### 主应用 (`main.go`)

应用配置了以下内容：
- **默认中间件**: Gin 默认包含日志和恢复中间件
- **路由定义**: `/ping` 端点返回 JSON 响应
- **服务器启动**: 监听 `0.0.0.0:8080`

### Gin 特性

- **快速**: 基于 httprouter，性能优异
- **中间件支持**: 丰富的中间件生态系统
- **JSON 绑定**: 自动 JSON 序列化/反序列化
- **路由组**: 支持路由分组和嵌套
- **参数绑定**: 支持 URL 参数、查询参数、表单数据等

## 使用示例

### 使用 curl 测试

```bash
# 测试 ping 端点
curl http://localhost:8080/ping
```

### 扩展示例

可以轻松添加更多路由：

```go
r.GET("/users", func(c *gin.Context) {
    c.JSON(200, gin.H{
        "users": []string{"Alice", "Bob"},
    })
})

r.POST("/users", func(c *gin.Context) {
    var user User
    if err := c.ShouldBindJSON(&user); err != nil {
        c.JSON(400, gin.H{"error": err.Error()})
        return
    }
    c.JSON(201, user)
})
```

## 开发

### 运行测试

```go
// 可以添加测试文件
// main_test.go
package main

import (
    "net/http"
    "net/http/httptest"
    "testing"
    "github.com/gin-gonic/gin"
    "github.com/stretchr/testify/assert"
)

func TestPing(t *testing.T) {
    gin.SetMode(gin.TestMode)
    r := gin.New()
    r.GET("/ping", func(c *gin.Context) {
        c.JSON(http.StatusOK, gin.H{"message": "pong"})
    })

    w := httptest.NewRecorder()
    req, _ := http.NewRequest("GET", "/ping", nil)
    r.ServeHTTP(w, req)

    assert.Equal(t, 200, w.Code)
    assert.Contains(t, w.Body.String(), "pong")
}
```

运行测试：
```bash
go test
```

## 部署

### 构建生产版本

```bash
# 构建优化版本
go build -ldflags="-s -w" -o gin-demo

# 设置环境变量（生产模式）
export GIN_MODE=release
```

### Docker 部署

```dockerfile
FROM golang:1.25-alpine AS builder
WORKDIR /app
COPY go.mod go.sum ./
RUN go mod download
COPY . .
RUN go build -o gin-demo

FROM alpine:latest
RUN apk --no-cache add ca-certificates
WORKDIR /root/
COPY --from=builder /app/gin-demo .
CMD ["./gin-demo"]
```

## 参考资源

- [Gin 官方网站](https://gin-gonic.com/)
- [Gin GitHub 仓库](https://github.com/gin-gonic/gin)
- [Gin 文档](https://gin-gonic.com/docs/)
