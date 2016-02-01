# 上下文

通过 Context 可以获取到 *Request 和 ResopnseWriter 。同时还有一些方便的函数可以进行操作。

## `Req()`
获取到*Request对象。

## `Forms()`
可以获取表单提交内容，具体参见 [Forms](ZH_Forms)

## `Cookies()`
获取到Cookies对象并进行操作。

## `SecureCookies()`
获取到安全Cookie对象并进行操作。

## `ServeFile()`
将文件发送给浏览器

## `ServeJson()`
发送Json数据到浏览器

## `ServeXml()`
发送Xml数据到浏览器

## `Download()`
下载某个文件

## `SaveToFile()`
将上传文件保存到本地磁盘

## `Params()`
获取路由匹配参数

## `Action()`
如果执行体是结构体，则返回当前执行的结构体对象。否则返回nil。

## `Redirect()`
重定向

## `NotModified()`
返回304

## `Unauthorized()`
返回未认证

## `NotFound()`
返回404

## `Abort()`
自定义错误返回
