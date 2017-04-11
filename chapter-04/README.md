# 中间件机制

Handler 是tango的中间件。在tango中，几乎所有的事情都由中间件来完成。撰写一个你自己的中间件非常简单，并且我们鼓励您只加载需要的中间件。

tango的中间件只需要符合以下接口即可。

```Go
type Handler interface {
    Handle(*tango.Context)
}
```

同时，tango也提供了```tango.HandlerFunc```，以方便你将一个函数包装为中间件。比如：

```Go
func MyHandler() tango.HandlerFunc {
    return func(ctx *tango.Context) {
        fmt.Println("this is my first tango handler")
        ctx.Next()
    }
}

t := tango.Classic()
t.Use(MyHandler())
t.Run()
```

正常的形式也可以是:

```
type HelloHandler struct {}
func (HelloHandler) Handle(ctx *tango.Context) {
    fmt.Println("before")
    ctx.Next()
    fmt.Println("after")
}

t := tango.Classic()
t.Use(new(HelloHandler))
t.Run()
```

当然，你可以直接将一个包含tango.Context指针的函数作为中间件，如：

```Go
tg.Use(func(ctx *tango.Context){
    fmt.Println("before")
    ctx.Next()
    fmt.Println("after")
})
```

为了和标准库兼容，tango通过UseHandler支持http.Handler作为中间件，如：

```Go
tg.UseHandler(http.Handler(func(resp http.ResponseWriter, req *http.Request) {

}))
```

老的中间件会被action被匹配之前进行调用。

# Call stack
以下是中间件的调用顺序图：

```
tango.ServeHttp
|--Handler1
      |--Handler2
            |-- ...HandlerN
                      |---Action(If matched)
                ...HandlerN--|
         Handler2 ----|
   Handler1--|
(end)--|
```

在中间件中，您的中间件代码可以在```Next()```被调用之前或之后执行，Next表示执行下一个中间件或Action被执行（如果url匹配的话）。如果不调用Next，那么当前请求将会被立即停止，之后的所有代码将不会被执行。
