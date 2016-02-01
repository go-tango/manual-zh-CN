Flash中间件为你在两次请求之间共享变量提供方便，他是一个 [Tango](https://github.com/lunny/tango) 的中间件。

Flash中间件是一个基于 [session](https://github.com/tango-contrib/session) 的封装。

## 安装

    go get github.com/tango-contrib/flash

## 示例

```Go

import "github.com/tango-contrib/session"

type FlashAction struct {
    flash.Flash
}

func (x *FlashAction) Get() {
    x.Flash.Set("test", "test")
}

func (x *FlashAction) Post() {
   x.Flash.Get("test").(string) == "test"
}

func main() {
    t := tango.Classic()
    sessions := session.Sessions()
    t.Use(flash.Flashes(sessions))
    t.Any("/", new(FlashAction))
    t.Run()
}
```
