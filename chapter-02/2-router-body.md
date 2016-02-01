## 路由执行体

Tango 支持 5 种形式的函数或结构体方法作为执行体：

* func()
* func(http.ResponseWriter, *http.Request)
* func(*tango.Context)
* func(http.Response.Writer)
* func(*http.Request)
* struct.Get()

* func()
```Go
t := tango.Classic()
t.Get("/", func() string {
    return "hello tango"
})
```

* func(http.ResponseWriter, *http.Request)
```Go
t := tango.Classic()
t.Post("/", func(w http.ResponseWriter, *http.Request) {
    w.Write([]byte("hello tango"))
})
```

* func(http.ResponseWriter)
```Go
t := tango.Classic()
t.Post("/", func(w http.ResponseWriter) {
    w.Write([]byte("hello tango"))
})
```

* func(*http.Request)
```Go
t := tango.Classic()
t.Post("/", func(req *http.Request) string {
    return req.FormValue("key")
})
```

* func(*tango.Context)
```Go
t := tango.Classic()
t.Get("/:name", func(ctx *tango.Context) {
    ctx.Write([]byte("hello " + ctx.Params().Get(":name")))
})
```

* structs
```Go
type Action struct {}
func (a *Action) Get() string {
    return "haha"
}
t := tango.Classic()
t.Get("/:name", new(Action))
```
