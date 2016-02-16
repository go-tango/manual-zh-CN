# Session NoDB Store

## 安装

    go get github.com/tango-contrib/session-nodb

## Example

```Go
package main

import (
    "github.com/lunny/tango"
    "github.com/tango-contrib/session"
    "github.com/tango-contrib/session-nodb"
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
    store, _ := nodbstore.New(nodbstore.Options{
        Path:    "./nodbstore",
        DbIndex: 0,
        MaxAge:  30 * time.Minute,
    })
    o.Use(session.New(session.Options{
        Store: store,
        }))
    o.Get("/", new(SessionAction))
}
```
