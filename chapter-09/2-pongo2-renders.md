# Pongo2模板引擎

Tpongo2 中间件是 [pongo2](https://github.com/flosch/pongo2).**v3** 模板引擎的 [Tango](https://github.com/lunny/tango) 支持。

## 安装

    go get github.com/tango-contrib/tpongo2

## 示例

```Go
package main

import (
    "github.com/lunny/tango"
    "gopkg.in/flosch/pongo2.v3"
    "github.com/tango-contrib/tpongo2"
)

type RenderAction struct {
    tpango2.Renderer
}

func (a *RenderAction) Get() error {
    return a.RenderString("Hello {{ name }}!", pongo2.Context{
        "name": "tango",
    })
}
//模板有发动自动重载
var  options = struct {
	TemplatesDir string
	Reload       bool
	Suffix       string
}{
	Reload: true,
	TemplatesDir: "./templates",
	Suffix:".html",
}
func main() {
    o := tango.Classic()
    o.Use(tpango2.New(options))//加入自动重载
    o.Get("/", new(RenderAction))
}
```
