# 路由匹配

## Tango支持4种形式的路由匹配规则

* 静态路由
```Go
tg.Get("/", new(Action))
tg.Get("/static", new(Action))
```
匹配 URL：/ 到 Action结构体的Get函数
匹配 URL：/static 到 Action结构体的Get函数


* 命名路由
```Go
tg.Get("/:name", new(Action))
tg.Get("/(:name)", new(Action))
tg.Get("/:name1/:name2", new(Action))
tg.Get("/:name1-:name2", new(Action))
tg.Get("/(:name1)sss(:name2)", new(Action))
```

* 通配路由
```Go
tg.Get("/*name", new(Action))
tg.Get("/ttt/*name", new(Action))
tg.Get("/sss(*name)", new(Action))
```
路由参数支持*通配符


* 正则路由

```Go
tg.Get("/(:name.*)", new(Action))
tg.Get("/(:name[0-9]+)", new(Action))
tg.Get("/(:name1)-(:name2[0-9]+)", new(Action))
```
路由参数支持正则约束，不符合条件的参数会抛出404错误

## 路由定义

除了可以用Get Post等方法来定义外，也可以使用Route来绑定路由，形式更灵活

* GET请求
```Go
t.Route("GET", "/", new(Action))
```

* 自定义方法名
```Go
type Action struct {}
func (Action) MyPost() {}

t.Route("POST:MyPost", "/", new(Action))
```

* 一次定义多个
```Go
t.Route([]string{"GET:MyGet", "POST"}, "/", new(Action))
```

* 自定义多个
```Go
t.Route(map[string]string{"GET":"MyGet", "POST":"MyPost"}, "/", new(Action))
```

## 路由优先级

* 静态路由和其它形式的路由都匹配时，静态路由优先，跟添加的顺序无关，如：
```Go
/abc
/(:name)
```
那么对于请求```/abc```，则```/abc```会匹配

* 多个动态路由均匹配时，先出现静态部分的路由优先，如：
```Go
"/(:name1)/(:name1)abc"
"/(:name1)/abc(:name1)abc(:name2)abc"
```
那么对于请求```/abc/abc123abc123abc```，则```/(:name1)/abc(:name1)abc(:name2)abc```会匹配

* 其它路由之间根据添加的顺序，先添加的优先。

## 路由参数

请求路径匹配后的参数可以通过*Context获得：
* 命名路由和正则路由: `ctx.Param(":name")` 或者 `ctx.Param("name")`
* 通配路由：`ctx.Param("*name")`

## GET参数获取实例

* 访问：http://xx.com/api/1.0/users?name=111  
* 注：name需小写，首字母大写取不到

```Go
import "github.com/tango-contrib/binding"

type Getuser struct {
  	tango.JSON
	tango.Ctx
	binding.Binder
}

func (this *Getuser) Get() interface{} {
	type GetStruct struct {
		Name string
	}
	var data GetStruct
	err := this.Bind(&data)
	if err != nil {
		return map[string]interface{}{
			"res":    "no",
			"msg":    "get param error",
			"err:":   err,
			"status": 500,
		}
	}
  fmt.Println(data.Name)//取值
  .....
  }
  
  //路由
  func main() {
	t := tango.Classic()
	t.Use(binding.Bind())
    t.Get("/api/1.0/users", new(Getuser))
    ......
```

##POST参数获取实例:JSON格式
* curl -H "Content-Type: application/json" -X POST  --Data '{ "Uid": "7","Aim_sn":"123456"}' -k https://xx.cn/inapi/1.0/setaiport

```Go
type Settid struct {
  	tango.JSON
	tango.Ctx
}

func (this *Settid) Post() interface{} {
	type jsondata struct {
		Uid    string `json:"Uid"`
		Tid    string `json:"Tid"`
		Aim_sn string `json:"Aim_sn"`
	}
	var data jsondata
	err = this.DecodeJSON(&data)
	if err != nil {
		return map[string]interface{}{
			"res":    "no",
			"msg":    "DecodeJSON error",
			"err:":   err,
			"status": 500,
		}
	}
  fmt.Println(data.Uid)//取值
  .....
  }
  
  //路由
  func main() {
    t := tango.Classic()
    t.Post("/inapi/1.0/setaiport", new(Settid))
    ...
```
