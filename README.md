# Gin Demo

A Go Web application demo project based on the [Gin](https://gin-gonic.com/) framework. Gin is an HTTP Web framework written in Go with a Martini-like API but with better performance.

## Tech Stack

- **Go**: 1.25.3
- **Gin**: 1.11.0

## Project Structure

```
gin-demo/
├── main.go              # Application entry point, route and handler definitions
├── go.mod               # Go module dependencies
├── go.sum               # Dependency checksums
├── index.http           # HTTP request test file
└── README.md
```

## Features

- Basic route handling
- JSON responses
- Middleware support (logging and recovery)
- Clean API design

## Quick Start

### Prerequisites

- Go 1.25.3 or higher

### Installation and Running

```bash
# Install dependencies
go mod download

# Run project
go run main.go
```

The service will start at `http://localhost:8080` (Gin default port).

### Build Executable

```bash
# Build
go build -o gin-demo

# Run
./gin-demo
```

## API Endpoints

### Ping Endpoint
```http
GET http://localhost:8080/ping
```
Response example:
```json
{
  "message": "pong"
}
```

## Code Description

### Main Application (`main.go`)

The application is configured with:
- **Default middleware**: Gin includes logging and recovery middleware by default
- **Route definition**: `/ping` endpoint returns JSON response
- **Server startup**: Listens on `0.0.0.0:8080`

### Gin Features

- **Fast**: Based on httprouter with excellent performance
- **Middleware support**: Rich middleware ecosystem
- **JSON binding**: Automatic JSON serialization/deserialization
- **Route groups**: Support for route grouping and nesting
- **Parameter binding**: Support for URL parameters, query parameters, form data, etc.

## Usage Examples

### Test with curl

```bash
# Test ping endpoint
curl http://localhost:8080/ping
```

### Extension Examples

You can easily add more routes:

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

## Development

### Run Tests

```go
// You can add test file
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

Run tests:
```bash
go test
```

## Deployment

### Build Production Version

```bash
# Build optimized version
go build -ldflags="-s -w" -o gin-demo

# Set environment variable (production mode)
export GIN_MODE=release
```

### Docker Deployment

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

## References

- [Gin Official Website](https://gin-gonic.com/)
- [Gin GitHub Repository](https://github.com/gin-gonic/gin)
- [Gin Documentation](https://gin-gonic.com/docs/)
