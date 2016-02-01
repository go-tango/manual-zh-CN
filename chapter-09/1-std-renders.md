# 模板渲染

Renders中间件是一个Go模板引擎的 [Tango](https://github.com/lunny/tango) 中间件。

## 安装

    go get github.com/tango-contrib/renders

## 示例

```Go
type RenderAction struct {
    renders.Renderer
}

func (x *RenderAction) Get() {
    x.Render("test.html", renders.T{
        "test": "test",
    })
}

func main() {
    t := tango.Classic()
    t.Use(renders.New(renders.Options{
        Reload: true,
        Directory: "./templates",
    }))
}
```
