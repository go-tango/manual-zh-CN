# Basic Auth 中间件

Basicauth 中间件提供了Http Basic认证，是一个 [Tango](https://github.com/lunny/tango) 的中间件。

## 安装

    go get github.com/tango-contrib/basicauth

## 示例

```Go
type AuthAction struct {}
func (a *AuthAction) Get() string {
    return "200"
}

func main() {
    tg := tango.Classic()
    tg.Use(basicauth.New(user, pass))
    tg.Get("/", new(AuthAction))
}
```

如果不希望某个Action进行认证，则只要把basiauth.NoAuth作为匿名成员即可。
```Go
type NoAuthAction struct {
    basicauth.NoAuth
}
func (a *NoAuthAction) Get() string {
    return "200"
}

func main() {
    tg := tango.Classic()
    tg.Use(basicauth.New(user, pass))
    tg.Get("/", new(NoAuthAction))
}
```
