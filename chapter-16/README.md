# 验证码中间件

Captcha中间件提供了验证码服务，他是一个 [Tango](https://github.com/lunny/tango) 的插件。

### 安装

	go get github.com/tango-contrib/captcha

## 示例

```go
package main

import (
	"github.com/lunny/tango"
	"github.com/tango-contrib/captcha"
	"github.com/tango-contrib/renders"
)

type CaptchaAction struct {
	captcha.Captcha
	renders.Renderer
}

func (c *CaptchaAction) Get() error {
	return c.Render("captcha.html", renders.T{
		"captcha": c.CreateHtml(),
	})
}

func (c *CaptchaAction) Post() string {
	if c.Verify() {
		return "true"
	}
	return "false"
}

func main() {
	t := tango.Classic()
	t.Use(captcha.New(), renders.New())
	t.Any("/", new(CaptchaAction))
	t.Run()
}
```

默认的验证码是保存在内存中，并保留120秒，当然也可以自定义：

```go
package main

import (
	"github.com/lunny/tango"
	"github.com/tango-contrib/cache"
	"github.com/tango-contrib/captcha"
	"github.com/tango-contrib/renders"
)

type CaptchaAction struct {
	captcha.Captcha
	renders.Renderer
}

func (c *CaptchaAction) Get() error {
	return c.Render("captcha.html", renders.T{
		"captcha": c.CreateHtml(),
	})
}

func (c *CaptchaAction) Post() string {
	if c.Verify() {
		return "true"
	}
	return "false"
}

func main() {
  	t := tango.Classic()
	c := cache.New(cache.Options{
		Adapter: "memory",
		Interval: 120,
	})
	t.Use(renders.New(), captcha.New(captcha.Options{Caches: c}))
	t.Any("/", new(CaptchaAction))
	t.Run()
}
```

如果希望使用Redis或者其他来保存，也可以：

```Go
package main

import (
	"github.com/lunny/tango"
	"github.com/tango-contrib/cache"
	_ "github.com/tango-contrib/cache-redis"
	"github.com/tango-contrib/captcha"
	"github.com/tango-contrib/renders"
)

type CaptchaAction struct {
	captcha.Captcha
	renders.Renderer
}

func (c *CaptchaAction) Get() error {
	return c.Render("captcha.html", renders.T{
		"captcha": c.CreateHtml(),
	})
}

func (c *CaptchaAction) Post() string {
	if c.Verify() {
		return "true"
	}
	return "false"
}

func main() {
	t := tango.Classic()
	c := cache.New(cache.Options{
		Adapter:       "redis",
		AdapterConfig: "addr=:6379,prefix=cache:",
	})
	t.Use(renders.New(), captcha.New(captcha.Options{Caches: c}))
	t.Any("/", new(CaptchaAction))
	t.Run()
}
```

```html
<!-- templates/captcha.tmpl -->
{{.captcha}}
```

## 选项

`captcha.Options` 有很多可以选择的配置，当然这些配置都有缺省值：

```go
// ...
t.Use(captcha.New(captcha.Options{
	Caches: cache,
	URLPrefix:			"/captcha/", 	// URL prefix of getting captcha pictures.
	FieldIdName:		"captcha_id", 	// Hidden input element ID.
	FieldCaptchaName:	"captcha", 		// User input value element name in request form.
	ChallengeNums:		6, 				// Challenge number.
	Width:				240,			// Captcha image width.
	Height:				80,				// Captcha image height.
	Expiration:			600, 			// Captcha expiration time in seconds.
	CachePrefix:		"captcha_", 	// Cache key prefix captcha characters.
}))
// ...
```
