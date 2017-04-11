# Events 事件

## 安装

```
go get github.com/tango-contrib/events
```

## 使用
Events 中间件让你可以在方法体执行前和执行后执行相关的代码，如：

```Go
type EventAction struct {
	tango.Ctx
}

func (c *EventAction) Get() {
	c.Write([]byte("get"))
}

func (c *EventAction) Before() {
	c.Write([]byte("before "))
}

func (c *EventAction) After() {
	c.Write([]byte(" after"))
}

func main() {
    t := tango.Classic()
    t.Use(events.Events())
    t.Get("/", new(EventAction))
    t.Run()
}
```
