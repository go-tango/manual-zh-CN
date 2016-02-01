# 压缩

Tango拥有一个默认的压缩中间件，可以按照扩展名来进行文件的压缩。同时，你也可以要求某个Action自动或强制使用某种压缩。比如：

```Go
type CompressExample struct {
    tango.Compress // 添加这个匿名结构体，要求这个结构体的方法进行自动检测压缩
}

func (CompressExample) Get() string {
    return fmt.Sprintf("This is a auto compress text")
}

o := tango.Classic()
o.Get("/", new(CompressExample))
o.Run()
```
以上代码默认会检测浏览器是否支持压缩，如果支持，则看是否支持gzip，如果支持gzip，则使用gzip压缩，如果支持deflate，则使用deflate压缩。

```Go
type GZipExample struct {
    tango.GZip // add this for ask compress to GZip, if accept-encoding has no gzip, then not compress
}

func (GZipExample) Get() string {
    return fmt.Sprintf("This is a gzip compress text")
}

o := tango.Classic()
o.Get("/", new(GZipExample))
o.Run()
```
以上代码默认会检测浏览器是否支持gzip压缩，如果支持gzip，则使用gzip压缩，否则不压缩。

```Go
type DeflateExample struct {
    tango.Deflate // add this for ask compress to Deflate, if not support then not compress
}

func (DeflateExample) Get() string {
    return fmt.Sprintf("This is a deflate compress text")
}

o := tango.Classic()
o.Get("/", new(DeflateExample))
o.Run()
```
以上代码默认会检测浏览器是否支持deflate压缩，如果支持deflate，则使用deflate压缩，否则不压缩。
