# Session

Session 是一个 [Tango](https://github.com/lunny/tango) 的 session 中间件.

## 后台

当前Session支持如下几种后台进行Session内容的存储:

* Memory - 将Session保存在内存中，这个是默认值
* [Redis](http://github.com/tango-contrib/session-redis) - 使用redis服务器进行Session保存
* [Ledis](http://github.com/tango-contrib/session-ledis) - 使用ledis服务器进行Session保存
* [nodb](http://github.com/tango-contrib/session-nodb) - 使用nodb文件来保存Session
* [ssdb](http://github.com/tango-contrib/session-ssdb) - 使用ssdb服务器来保存Session

**注意，如果使用永久存储，并且Session里面存储了结构体，应在程序启动时进行结构体的gob注册**

## 安装

    go get github.com/tango-contrib/session

## 例子

```Go
package main

import (
    "github.com/lunny/tango"
    "github.com/tango-contrib/session"
)

type SessionAction struct {
    session.Session
}

func (a *SessionAction) Get() string {
    a.Session.Set("test", "1")
    return a.Session.Get("test").(string)
}

func main() {
    o := tango.Classic()
    o.Use(session.New(session.Options{
        MaxAge:time.Minute * 20,
        }))
    o.Get("/", new(SessionAction))
}
```

如果使用其它后台，则：
```Go
func main() {
    o := tango.Classic()
    o.Use(session.New(session.Options{
        MaxAge:time.Minute * 20,
        Store: redistore.New(Options{
			Host:    "127.0.0.1",
			DbIndex: 0,
			MaxAge:  30 * time.Minute,
		},
        }))
    o.Get("/", new(SessionAction))
}
```
