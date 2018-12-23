---
title: Go Validate 工具库
permalink: /docs/zh/validate
key:
---

[![GoDoc](https://godoc.org/github.com/gookit/validate?status.svg)](https://godoc.org/github.com/gookit/validate)
[![Build Status](https://travis-ci.org/gookit/validate.svg?branch=master)](https://travis-ci.org/gookit/validate)
[![Coverage Status](https://coveralls.io/repos/github/gookit/validate/badge.svg?branch=master)](https://coveralls.io/github/gookit/validate?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/gookit/validate)](https://goreportcard.com/report/github.com/gookit/validate)

<iframe src="https://ghbtns.com/github-btn.html?user=gookit&repo=validate&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>

Go通用的数据验证与过滤库，使用简单，内置大部分常用验证器、过滤器，支持自定义消息、字段翻译。

- 支持验证Map，Struct，Request（Form，JSON，url.Values, UploadedFile）数据
- 简单方便，支持前置验证检查, 支持添加自定义验证器
- 支持将规则按场景进行分组设置。不同场景验证不同的字段
- 支持在进行验证前对值使用过滤器进行净化过滤，查看 [内置过滤器](#built-in-filters)
- 已经内置了超多（> 60 个）常用的验证器，查看 [内置验证器](#built-in-validators)
- 方便的获取错误信息，验证后的安全数据获取(只会收集有规则检查过的数据)
- 支持自定义每个验证的错误消息，字段翻译，消息翻译(内置`en` `zh-CN`)
- 完善的单元测试，测试覆盖率 > 90%

> 受到这些项目的启发 [albrow/forms](https://github.com/albrow/forms) 和 [asaskevich/govalidator](https://github.com/asaskevich/govalidator). 非常感谢它们

## GoDoc

- [godoc for github](https://godoc.org/github.com/gookit/validate)

## 验证结构体(Struct)

```go
package main

import "fmt"
import "time"
import "github.com/gookit/validate"

// UserForm struct
type UserForm struct {
    Name     string    `validate:"required|minLen:7"`
    Email    string    `validate:"email"`
    Age      int       `validate:"required|int|min:1|max:99"`
    CreateAt int       `validate:"min:1"`
    Safe     int       `validate:"-"`
    UpdateAt time.Time `validate:"required"`
    Code     string    `validate:"customValidator"` // 使用自定义验证器
}

// CustomValidator 定义在结构体中的自定义验证器
func (f UserForm) CustomValidator(val string) bool {
    return len(val) == 4
}

// Messages 您可以自定义验证器错误消息
func (f UserForm) Messages() map[string]string {
    return validate.MS{
        "required": "oh! the {field} is required",
        "Name.required": "message for special field",
    }
}

// Translates 你可以自定义字段翻译
func (f UserForm) Translates() map[string]string {
    return validate.MS{
        "Name": "用户名称",
        "Email": "用户Email",
    }
}

func main() {
    u := &UserForm{
        Name: "inhere",
    }
    
    // 创建 Validation 实例
    v := validate.Struct(u)
    // v := validate.New(u)

    if v.Validate() { // 验证成功
        // do something ...
    } else {
        fmt.Println(v.Errors) // 所有的错误消息
        fmt.Println(v.Errors.One()) // 返回随机一条错误消息
        fmt.Println(v.Errors.Field("Name")) // 返回该字段的错误消息
    }
}
```

## 验证`Map`数据

```go
package main

import "fmt"
import "time"
import "github.com/gookit/validate"

func main()  {
    m := map[string]interface{}{
        "name":  "inhere",
        "age":   100,
        "oldSt": 1,
        "newSt": 2,
        "email": "some@email.com",
    }

    v := validate.Map(m)
    // v := validate.New(m)
    v.AddRule("name", "required")
    v.AddRule("name", "minLen", 7)
    v.AddRule("age", "max", 99)
    v.AddRule("age", "min", 1)
    v.AddRule("email", "email")
    
    // 也可以这样，一次添加多个验证器
    v.StringRule("age", "required|int|min:1|max:99")
    v.StringRule("name", "required|minLen:7")

    // 设置不同场景验证不同的字段
    // v.WithScenes(map[string]string{
    //   "create": []string{"name", "email"},
    //   "update": []string{"name"},
    // })
    
    if v.Validate() { // validate ok
        // do something ...
    } else {
        fmt.Println(v.Errors) // all error messages
        fmt.Println(v.Errors.One()) // returns a random error message text
    }
}
```

## 验证请求

传入 `*http.Request`，快捷方法 `FromRequest()` 就会自动根据请求方法和请求数据类型收集相应的数据

- `GET/DELETE/...` 等，会搜集 url query 数据
- `POST/PUT/PATCH` 并且类型为 `application/json` 会搜集JSON数据
- `POST/PUT/PATCH` 并且类型为 `multipart/form-data` 会搜集表单数据，同时会收集文件上传数据
- `POST/PUT/PATCH` 并且类型为 `application/x-www-form-urlencoded` 会搜集表单数据

```go
package main

import "fmt"
import "time"
import "net/http"
import "github.com/gookit/validate"

func main()  {
    handler := http.HandlerFunc(func(w http.ResponseWriter, r *http.Request) {
        data, err := validate.FromRequest(r)
        if err != nil {
            panic(err)
        }
        
        v := data.Create()
        // setting rules
        v.FilterRule("age", "int") // convert value to int
        
        v.AddRule("name", "required")
        v.AddRule("name", "minLen", 7)
        v.AddRule("age", "max", 99)
        
        if v.Validate() { // validate ok
            // do something ...
        } else {
            fmt.Println(v.Errors) // all error messages
            fmt.Println(v.Errors.One()) // returns a random error message text
        }
    })
    
    http.ListenAndServe(":8090", handler)
}
```

## 常用方法

快速创建 `Validation` 实例：

- `Request(r *http.Request) *Validation`
- `JSON(s string, scene ...string) *Validation`
- `Struct(s interface{}, scene ...string) *Validation`
- `Map(m map[string]interface{}, scene ...string) *Validation`
- `New(data interface{}, scene ...string) *Validation` 

快速创建 `DataFace` 实例：

- `FromMap(m map[string]interface{}) *MapData`
- `FromStruct(s interface{}) (*StructData, error)`
- `FromJSON(s string) (*MapData, error)`
- `FromJSONBytes(bs []byte) (*MapData, error)`
- `FromURLValues(values url.Values) *FormData`
- `FromRequest(r *http.Request, maxMemoryLimit ...int64) (DataFace, error)`

> 通过 `DataFace` 创建 `Validation` 

```go
d := FromMap(map[string]interface{}{"key": "val"})
v := d.Validation()
```

`Validation` 常用方法：

- Validation.`AtScene(scene string) *Validation` 设置当前验证场景名
- Validation.`Filtering() bool` 应用所有过滤规则
- Validation.`Validate() bool` 应用所有验证和过滤规则
- Validation.`SafeData() map[string]interface{}` 获取所有经过验证的数据

## 欢迎star

- **[github](https://github.com/gookit/validate)**
- [gitee](https://gitee.com/inhere/validate)

## 参考项目

- https://github.com/albrow/forms
- https://github.com/asaskevich/govalidator
- https://github.com/go-playground/validator
- https://github.com/inhere/php-validate

## License

**[MIT](LICENSE)**
