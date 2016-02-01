# 调试中间件

Debug中间件为你调试请求和返回提供方便，它是一个 [Tango](https://github.com/lunny/tango) 的中间件。

## 安装

    go get github.com/tango-contrib/debug

## 示例

```Go
type DebugAction struct {
    tango.Ctx
}

func (c *DebugAction) Get() {
    c.Write([]byte("get"))
}

func main() {
    t := tango.Classic()
    t.Use(debug.Debug())
    t.Get("/", new(DebugAction))
    t.Run()
}
```

当你运行时，你将会在控制台或log文件中发现请求和响应的头和具体信息。

```
[tango] 2015/03/04 06:44:06 [Debug] debug.go:53 [debug] request: GET http://localhost:3000/
[tango] 2015/03/04 06:44:06 [Debug] debug.go:55 [debug] head: map[]
[tango] 2015/03/04 06:44:06 [Debug] debug.go:66 [debug] ----------------------- end request
[tango] 2015/03/04 06:44:06 [Debug] debug.go:78 [debug] response ------------------ 200
[tango] 2015/03/04 06:44:06 [Debug] debug.go:80 [debug] head: map[]
[tango] 2015/03/04 06:44:06 [Debug] debug.go:83 [debug] body: debug
[tango] 2015/03/04 06:44:06 [Debug] debug.go:85 [debug] ----------------------- end response
```
