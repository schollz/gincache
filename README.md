# Cache gin's middleware

[![Build Status](https://travis-ci.org/schollz/gincache.svg)](https://travis-ci.org/schollz/gincache)
[![codecov](https://codecov.io/gh/schollz/gincache/branch/master/graph/badge.svg)](https://codecov.io/gh/schollz/gincache)
[![Go Report Card](https://goreportcard.com/badge/github.com/schollz/gincache)](https://goreportcard.com/report/github.com/schollz/gincache)
[![GoDoc](https://godoc.org/github.com/schollz/gincache?status.svg)](https://godoc.org/github.com/schollz/gincache)

Gin middleware/handler to enable Cache.

## Usage

### Start using it

Download and install it:

```sh
$ go get github.com/schollz/gincache
```

Import it in your code:

```go
import "github.com/schollz/gincache"
```

### Canonical example:

See the [example](example/example.go)

```go
package main

import (
	"fmt"
	"time"

	"github.com/schollz/gincache"
	"github.com/schollz/gincache/persistence"
	"github.com/gin-gonic/gin"
)

func main() {
	r := gin.Default()

	store := persistence.NewInMemoryStore(time.Second)
	
	r.GET("/ping", func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	})
	// Cached Page
	r.GET("/cache_ping", cache.CachePage(store, time.Minute, func(c *gin.Context) {
		c.String(200, "pong "+fmt.Sprint(time.Now().Unix()))
	}))

	// Listen and Server in 0.0.0.0:8080
	r.Run(":8080")
}
```
