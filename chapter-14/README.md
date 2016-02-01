# Binding

Binding中间件提供了自动提取请求参数到结构体的映射和要求检查。他是一个 [Tango](https://github.com/lunny/tango) 的中间件。

## 安装

	go get github.com/tango-contrib/binding

## 示例

```Go
import (
    "github.com/lunny/tango"
    "github.com/tango-contrib/binding"
)

type Action struct {
    binding.Binder
}

type MyStruct struct {
    Id int64
    Name string
}

func (a *Action) Get() string {
    var mystruct MyStruct
    errs := a.Bind(&mystruct)
    return fmt.Sprintf("%v, %v", mystruct, errs)
}

func main() {
    t := tango.Classic()
    t.Use(binding.Bind())
    t.Get("/", new(Action))
    t.Run()
}
```

访问 `/?id=1&name=2` 你将会查看到输入如下：
```
{1 sss}, []
```
