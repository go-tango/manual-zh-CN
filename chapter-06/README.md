# 静态文件

静态文件的服务，可以有两种方式来进行，一种是中间件，另一种是直接使用内置的Actions。

## 使用中间件

Static 让你用一行代码可以完成一个静态服务器。

```Go
func main() {
    t := tango.New(tango.Static())
    t.Run()
}
```

然后，将你的文件放到 `./public` 目录下，你就可以通过浏览器放问到他们。比如：
```
http://localhost/images/logo.png  --> ./public/images/logo.png
```

当然，你也可以加入你```basicauth```或者你自己的认证中间件，这样就变为了一个私有的文件服务器。
```Go
func main() {
    t := tango.New()
    t.Use(AuthHandler)
    t.Use(tango.Static())
    t.Run()
}
```
你也可以通过`StaticOptions`来改变Static的默认参数，比如：
```Go
t.Use(tango.Static(tango.StaticOptions{Prefix:"static"}))
```

## 内置Actions

## File Action

```Go
tg := tango.New()
tg.Get("/index.html", tango.File("./public/index.html"))
tg.Run()
```

然后在浏览器访问 `http://localhost:8000/index.html`。

## Dir Action

```Go
tg := tango.New()
tg.Get("/:name", tango.Dir("./public"))
tg.Run()
```

然后在浏览器访问 `http://localhost:8000/test.html`。
