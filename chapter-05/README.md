# Logger

Logger 是一个接口，默认使用 [https://github.com/lunny/log](https://github.com/lunny/log) 作为Log。你也可以实现你自己的Logger。

```Go
type Logger interface {
	Debugf(format string, v ...interface{})
	Debug(v ...interface{})
	Infof(format string, v ...interface{})
	Info(v ...interface{})
	Warnf(format string, v ...interface{})
	Warn(v ...interface{})
	Errorf(format string, v ...interface{})
	Error(v ...interface{})
}
```

传入自定义的Logger：

```Go
l := log.New(out, "[tango] ", log.Ldefault())
l.SetOutputLevel(log.Ldebug)
t := tango.Classic(l)
t.Run()
```
