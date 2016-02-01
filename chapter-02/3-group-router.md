# 路由分组

通过Group可以实现路由分组，Group 路由分组可以简化你的路由撰写：

有两种方法来使用`Group`:

第一种，创建Group对象，通过Group方法传入
```Go
g := tango.NewGroup()
g.Get("/1", func() string {
    return "/1"
})
g.Post("/2", func() string {
    return "/2"
})

o := tango.Classic()
o.Group("/api", g)
```
以上代码执行后，访问 `/api/1` 将执行函数1， `/api/2` 将执行函数2.

第二种方法，直接传入带Group为参数的函数：
```Go
o := tango.Classic()
o.Group("/api", func(g *tango.Group) {
    g.Get("/1", func() string {
	return "/1"
    })
    g.Post("/2", func() string {
	return "/2"
    })
})
```
以上代码执行后，访问 `/api/1` 将执行函数1， `/api/2` 将执行函数2.

同时， `Group` 也支持子 `Group`:
```Go
o := tango.Classic()
o.Group("/api", func(g *tango.Group) {
    g.Group("/v1", func(cg *tango.Group) {
        cg.Get("/1", func() string {
	    return "/1"
        })
        cg.Post("/2", func() string {
	    return "/2"
        })
    })
})
```
以上代码执行后，访问 `/api/v1/1` 将执行函数1， `/api/v1/2` 将执行函数2.

有时候，我们仅仅只是想逻辑上分组，而这些请求路径并没有共同的父路径，那么也是可以的，代码如下：
```Go
o := tango.Classic()
o.Group("", func(g *tango.Group) {
    g.Get("/1", func() string {
	return "/1"
    })
})

o.Group("", func(g *tango.Group) {
    g.Post("/2", func() string {
	return "/2"
    })
})
```
以上代码执行后，访问 `/1` 将执行函数1， `/2` 将执行函数2.

## 分组中间件

最后，我们也可以将中间件应用于Group之上，用group的Use方法。

```Go
o := tango.Classic()
o.Group("/api/v1", func(g *tango.Group) {
    g.Use(handlers...)
    g.Get("/1", func() string {
    return "/1"
    })
})
```
