# 多Tango实例

Dispacth中间件可以通过前缀来分派请求到不同的Tango中。

# 示例

```Go
import (
    "github.com/lunny/tango"
    "github.com/tango-contrib/dispatch"
)

func main() {
    logger := tango.NewLogger(os.Stdout)
    t1 := tango.NewWithLog(logger)
    t2 := tango.NewWithLog(logger)

    dispatch := dispatch.New(map[string]*tango.Tango{
        "/": t1,
        "/api/": t2,
    })

    t3 := tango.NewWithLog(logger)
    t3.Use(dispatch)
    t3.Run(":8000")
}
```
