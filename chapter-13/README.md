# XSRF

XSRF 是一个 [Tango](https://github.com/lunny/tango) 的 XSRF 中间件.

可以起到防止跨站请求伪造和表单重复提交的作用。

## 安装
    go get github.com/tango-contrib/xsrf

## 示例
```Go
type Action struct {
    render.Render
    xsrf.Checker
}

func (a *Action) Get() error {
    return a.Render("test.html", render.T{
        "XsrfFormHtml": a.XsrfFormHtml(),
    })
}

func (a *Action) Post() {
    //中间件会自动检查XSRF，如果校验失败会输出默认的错误信息
}

func main() {
    t := tango.Classic()
    t.Use(xsrf.New(expireTime))  //expireTime为XSRF值的刷新时间
    t.Run()
}
```

如果希望某个Action不进行校验XSRF，可以在Action结构体中使用`xsrf.NoCheck`：
```Go
type Action struct {
    xsrf.NoCheck
}
```

如果希望某个Action不自动校验XSRF，改为自己校验XSRF后输出自定义的XSRF错误信息，则需要实现中间件的`AutoCheck()`方法：
```Go
type Action struct {
    xsrf.Checker
}

func (a *Action) AutoCheck() bool {
    return false
}

func (a *Action) Post() {
    if a.IsValid() == false {
        //这里写XSRF校验失败的代码
    }
}
```

还可以在XSRF校验之后执行 `Renew()` 方法来生成新的XSRF值，起到防止表单刷新的作用：
```Go
type Action struct {
    xsrf.Checker
}

func (a *Action) AutoCheck() bool {
    return false
}

func (a *Action) Post() {
    if a.IsValid() == false {
        //这里写XSRF校验失败的代码
    }
    a.Renew()  //重新生成新的XSRF值，刷新表单时XSRF会校验失败
}
```
