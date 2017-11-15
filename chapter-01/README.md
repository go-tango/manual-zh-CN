# 开始

## 安装

安装Tango：

```go
go get github.com/lunny/tango
```

一个经典的Tango例子如下：

```go
package main

import (
    "errors"
    "github.com/lunny/tango"
)

type Action struct {
    tango.JSON
}

func (Action) Get() interface{} {
    if true {
        return map[string]string{
            "say": "Hello tango!",
        }
    }
    return errors.New("something error")
}

func main() {
    t := tango.Classic()
    t.Get("/", new(Action))
    t.Run()
    //指定监听端口
    //t.Run("0.0.0.0:8080")
    //HTTPS支持
    //t.RunTLS("server.crt","server.key")
}
```

然后在浏览器访问`http://localhost:8000`, 将会得到一个json返回
```
{"say":"Hello tango!"}
```

如果将上述例子中的 `true` 改为 `false`, 将会得到一个json返回
```
{"err":"something error"}
```

这段代码因为拥有一个内嵌的`tango.JSON`，所以返回值会被自动的转成Json

## 使用

`Tango` 对象是所有事情的开始。通常，我们只通过调用```tango.Classic()```就可以创建一个Tango对象，大多数情况都可以满足。如：

```Go
func main() {
    t := tango.Classic()
    t.Use(...)
    t.Get(...)
    t.Run()
}
```

默认的tango有一个自带的Log，日志输出是在屏幕，如果您想使用自定义的Logger，则可以
```Go
import "github.com/lunny/log"

l := log.New(out, "[tango] ", log.Ldefault())
l.SetOutputLevel(log.Ldebug)
t := tango.Classic(l)
```

Classic接受的Logger是一个接口，只要实现了Debug, Warn, Error等方法均可，因此您也可以采用自己的Logger组件。

当然可扩展性体现在你可以自定义，比如你只是想要提供一个静态文件服务，则：
```Go
func main() {
   t := tango.New(tango.Static(tango.StaticOptions{Prefix:"public"}))
    t.Run()
}
```
然后，你只需将要访问的文件放到 `./public` 目录下，就可以通过浏览器来访问了。

如果您希望使用第三方的Logger，那么可以调用以下函数。只要这个Logger实现了```tango.Logger```接口即可。

* `NewWithLog(Logger, ...tango.Handler)`
