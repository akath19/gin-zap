# Gin-Zap

[Gin](https://github.com/gin-gonic/gin) middleware for Logging with
[Zap](https://github.com/uber-go/zap), the structured, leveled logging framework from Uber

[![CircleCI](https://circleci.com/gh/akath19/gin-zap.svg?style=svg)](https://circleci.com/gh/akath19/gin-zap)
[![Go Report Card](https://goreportcard.com/badge/github.com/akath19/gin-zap)](https://goreportcard.com/report/github.com/akath19/gin-zap)
[![GoDoc](https://godoc.org/github.com/akath19/gin-zap?status.svg)](https://godoc.org/github.com/akath19/gin-zap)

## Usage
### Example
```go
package main

import (
    "flag"
    "time"

    "github.com/uber-go/zap"
    "github.com/akath19/gin-zap"
    "github.com/gin-gonic/gin"
)

func main() {
    //Gin Router
    router := gin.New()
    //Zap logger
    zap := zap.NewProduction()
    //Add middleware to Gin, requires sync duration & zap pointer
    router.Use(ginzap.Logger(3 * time.Second, zap))
    //Other gin configs
    router.Use(gin.Recovery())

    //Logger will work outside Gin as well
    zap.Warn("Warning")
    zap.Error("Error")
    zap.Info("Info")

    //Start Gin
    router.Run(":8080")
}
```
Gin-Zap will produces lines in the following way:

`[2018-08-06T14:27:43.001-0500]	WARN	[GIN]	{"statusCode": 404, "latency": "1.232µs", "clientIP": "::1", "method": "GET", "path": "/png", "error": ""}`

`2018-08-06T14:27:43.262-0500	WARN	[GIN]	{"statusCode": 404, "latency": "60.356µs", "clientIP": "::1", "method": "GET", "path": "/favicon.ico", "error": ""}`

`2018-08-06T14:27:43.267-0500	WARN	[GIN]	{"statusCode": 404, "latency": "1.029µs", "clientIP": "::1", "method": "GET", "path": "/favicon.ico", "error": ""}`

`2018-08-06T14:27:45.397-0500	INFO	[GIN]	{"statusCode": 200, "latency": "98.698µs", "clientIP": "::1", "method": "GET", "path": "/ping", "error": ""}`
`

## Contributing

All PRs are welcome!

## License

See [LICENSE](LICENSE) file.
