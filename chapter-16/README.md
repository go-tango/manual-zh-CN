# 验证码中间件

Captcha中间件提供了验证码服务，他是一个 [Tango](https://github.com/lunny/tango) 的插件。

### 安装

	go get github.com/tango-contrib/captcha

## 示例

```go
// main.go
import (
	"github.com/lunny/tango"
	"github.com/macaron-contrib/cache"
	"github.com/tango-contrib/captcha"
)

type CaptchaAction struct {
	captcha.Captcha
	renders.Renderer
}

func (c *CaptchaAction) Get() {
	c.Render("captcha.html", renders.T{
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
	c, _ := cache.NewCache("memory", `{"interval":120}`)
	t.Use(captcha.New(captcha.Options{}, c))
	t.Any("/", new(CaptchaAction))
	t.Run()
}
```

```html
<!-- templates/captcha.tmpl -->
{{.captcha}}
```

## 选项

`captcha.Captchaer` comes with a variety of configuration options:

```go
// ...
t.Use(captcha.New(captcha.Options{
	URLPrefix:			"/captcha/", 	// URL prefix of getting captcha pictures.
	FieldIdName:		"captcha_id", 	// Hidden input element ID.
	FieldCaptchaName:	"captcha", 		// User input value element name in request form.
	ChallengeNums:		6, 				// Challenge number.
	Width:				240,			// Captcha image width.
	Height:				80,				// Captcha image height.
	Expiration:			600, 			// Captcha expiration time in seconds.
	CachePrefix:		"captcha_", 	// Cache key prefix captcha characters.
}, cache))
// ...
```
