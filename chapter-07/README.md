# 返回值

根据函数或者结构体方法的返回值，returnHandle 插件将自动将内容写入到 ResponseWriter.
目前支持的返回值及对应的行为如下：

* `string`
返回string，将会把string转为[]byte同时写入到ResponseWriter

* `[]byte`
返回[]byte将会直接写入ResponseWriter

* `error`
返回错误，如果error不为nil, 则写入返回头500，内容为error.Error()

* `AbortError`
如果返回AbortError, 则写入返回头AbortError.Code，内容为AbortError.Error()

如果包含了匿名结构体tango.Json或者tango.Xml，则返回值的行为将会发生变化：
* `error`
返回值为error，如果是Json，则会生成```{"err": err.Error()}```的Json格式

* `map`, `slice`, `structs`
返回值为map，如果是Json，则会自动序列化为Json格式。

例子代码如下：
```Go
type Action struct {
    tango.Json
}

var i int
func (Action) Get() interface{} {
   if i == 0 {
       i = i + 1
       return map[string]interface{}{"i":i}
   }
   return errors.New("could not visit")
}

func main() {
    t := tango.Classic()
    t.Any("/", new(Action))
    t.Run()
}
```

当然返回值也可以加上HTTP状态，如：

```Go
type Action struct {
    tango.Json
}

var i int
func (Action) Get() (int, interface{}) {
   if i == 0 {
       i = i + 1
       return 200, map[string]interface{}{"i":i}
   }
   return 500, errors.New("could not visit")
}

func main() {
    t := tango.Classic()
    t.Any("/", new(Action))
    t.Run()
}
```
