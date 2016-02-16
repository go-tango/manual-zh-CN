# Session Tracker

Tracker是进行Session跟踪的方式，默认Session使用存储在Cookie里面的SessionID来进行跟踪，当然也可以使用HeaderTracker，例如：

```Go
sess := session.New(session.Options{
	Tracker: session.NewHeaderTracker(session.DefaultSessionIdName),
	MaxAge: sessionTimeout,
})
```

HeaderTracker会通过读取请求头部的Header中名为session.DefaultSessionIdName的字段进行Session跟踪。
