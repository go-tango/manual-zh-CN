# Session Ledis Store

## 安装

    go get github.com/tango-contrib/session-ledis

## Example

```Go
package main

import (
    "github.com/lunny/tango"
    "github.com/tango-contrib/session"
    "github.com/tango-contrib/session-ledis"
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
        Store: redistore.New(ledistore.Options{
                Host:    "127.0.0.1",
                DbIndex: 0,
                MaxAge:  30 * time.Minute,
            }),
        }))
    o.Get("/", new(SessionAction))
}
```
