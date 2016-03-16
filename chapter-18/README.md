# Cache 中间件

Cache 中间件让你的应用可以非常简单的将对象保存到各种临时或者永久存储中。包括 memory, file, Redis, Memcache, PostgreSQL, MySQL, Ledis and Nodb.

## 安装
```Go
go get github.com/tango-contrib/cache
```

### 默认使用memory存储
```Go
package main

import (
	"github.com/lunny/log"
	"github.com/lunny/tango"
	"github.com/tango-contrib/cache"
)

func main() {
	app := tango.Classic(log.Std)

	app.Use(cache.New())

	app.Get("/", new(Action))
	app.Run()
}

type Action struct {
	cache.Cache
}
func (this *Action) Get() interface{} {
	//写缓存，参数：键名，键值，生命期（秒）
	this.Cache.Put("test", "Hello Tango!", 20)
	//读取缓存
	return this.Cache.Get("test")
}
```

### 使用redis存储
```Go
package main

import (
	"github.com/lunny/log"
	"github.com/lunny/tango"
	"github.com/tango-contrib/cache"
	_ "github.com/tango-contrib/cache-redis"
)

func main() {
	app := tango.Classic(log.Std)

	cacheOptions := cache.Options{
		Adapter:       "redis",
		AdapterConfig: "addr=127.0.0.1:6379,prefix=cache:",
	}
	app.Use(cache.New(cacheOptions))

	app.Get("/", new(Action))
	app.Run()
}

type Action struct {
	cache.Cache
}
func (this *Action) Get() interface{} {
	this.Cache.Put("test", "Hello Tango!", 20)
	return this.Cache.Get("test")
}
```
