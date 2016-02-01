# Error Handler

所有的错误最终都会被 ```Tango.ErrHandler``` 进行处理。

你可以自定义你的错误处理方式来替代默认的。例如：

```Go
var (
	prefix = "<html><head>tango</head><body><div>"
	suffix = fmt.Sprintf("</div><div>version: %s</div></body></html>", tango.Version())
)
o := tango.Classic()
o.ErrHandler = tango.HandlerFunc(func(ctx *Context) {
    ctx.Write([]byte(prefix))
    switch res := ctx.Result.(type) {
        case tango.AbortError:
            ctx.WriteHeader(res.Code())
	    ctx.Write([]byte(res.Error()))
        case error:
	    ctx.WriteHeader(http.StatusInternalServerError)
	    ctx.Write([]byte(res.Error()))
	case []byte:
	    ctx.WriteHeader(http.StatusInternalServerError)
	    ctx.Write(res)
	case string:
	    ctx.WriteHeader(http.StatusInternalServerError)
	    ctx.Write([]byte(res))
	default:
	    ctx.WriteHeader(http.StatusInternalServerError)
	    ctx.Write([]byte(http.StatusText(http.StatusInternalServerError)))
    }
    ctx.Write([]byte(suffix))
})
```
