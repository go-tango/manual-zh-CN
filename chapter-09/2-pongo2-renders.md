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

func main() {
    o := tango.Classic()
    o.Use(tpango2.New())
    o.Get("/", new(RenderAction))
}
```
