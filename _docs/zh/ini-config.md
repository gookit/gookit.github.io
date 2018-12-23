---
title: Go INI 简单配置管理
permalink: /docs/zh/ini
key:
---

[![GoDoc](https://godoc.org/github.com/gookit/ini?status.svg)](https://godoc.org/github.com/gookit/ini)
[![Build Status](https://travis-ci.org/gookit/ini.svg?branch=master)](https://travis-ci.org/gookit/ini)
[![Coverage Status](https://coveralls.io/repos/github/gookit/ini/badge.svg?branch=master)](https://coveralls.io/github/gookit/ini?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/gookit/ini)](https://goreportcard.com/report/github.com/gookit/ini)

使用INI格式作为配置，配置数据的加载，管理，使用

- 使用简单(获取: `Int` `Bool` `String` `StringMap`, 设置: `SetInt` `SetBool` `SetString` ...)
- 支持多文件，数据加载
- 支持数据覆盖合并
- 支持解析 ENV 变量名
- 完善的单元测试(coverage > 90%)
- 支持变量参考，默认兼容Python的configParser格式 `%(VAR)s`

## 更多格式

如果你想要更多文件内容格式的支持，推荐使用 `gookit/config`

- [gookit/config](https://github/gookit/config) - 支持多种格式: `JSON`(default), `INI`, `YAML`, `TOML`, `HCL`

## Godoc

- [doc on gowalker](https://gowalker.org/github.com/gookit/ini)
- [godoc for github](https://godoc.org/github.com/gookit/ini)

## 快速使用

- 示例数据(`testdata/test.ini`):

```ini
# comments
name = inhere
age = 50
debug = true
hasQuota1 = 'this is val'
hasQuota2 = "this is val1"
can2arr = val0,val1,val2
shell = ${SHELL}
noEnv = ${NotExist|defValue}
nkey = val in default section

; comments
[sec1]
key = val0
some = value
stuff = things
varRef = %(nkey)s
```

### 载入数据

```go
package main

import (
    "github.com/gookit/ini"
)

// go run ./examples/demo.go
func main() {
    // config, err := ini.LoadFiles("testdata/tesdt.ini")
    // LoadExists 将忽略不存在的文件
    config, err := ini.LoadExists("testdata/test.ini", "not-exist.ini")
    if err != nil {
        panic(err)
    }
    
    // 加载更多，相同的键覆盖之前数据
    config.LoadStrings(`
age = 100
[sec1]
newK = newVal
some = change val
`)
    // fmt.Printf("%v\n", config.Data())
}
```

### 获取数据

- 获取整型

```go
age, ok := config.Int("age")
fmt.Print(ok, age) // true 100
```

- 获取布尔值

```go
val, ok := config.Bool("debug")
fmt.Print(ok, age) // true true
```

- 获取字符串

```go
name, ok := config.String("name")
fmt.Print(ok, name) // true inhere
```

- 获取section数据(string map)

```go
val, ok := config.StringMap("sec1")
fmt.Println(ok, val) 
// true map[string]string{"key":"val0", "some":"change val", "stuff":"things", "newK":"newVal"}
```

- 获取的值是环境变量

```go
value, ok := config.String("shell")
fmt.Printf("%v %q", ok, value)  // true "/bin/zsh"
```

- 通过key path来直接获取子级值

```go
value, ok := config.String("sec1.key")
fmt.Print(ok, value) // true val0
```

- 支持变量参考

```go
value, ok := config.String("sec1.varRef")
fmt.Printf("%v %q", ok, value)  // true "val in default section"
```

- 设置新的值

```go
// set value
config.Set("name", "new name")
name, ok = config.String("name")
fmt.Printf("%v %q", ok, value)  // true "new name"
```

## 变量参考解析

```ini
[portal] 
url = http://%(host)s:%(port)s/Portal
host = localhost 
port = 8080
```

启用变量解析后，将会解析这里的 `%(host)s` 并替换为相应的变量值 `localhost`：

```go
cfg := ini.New()
// 启用变量解析
cfg.WithOptions(ini.ParseVar)

fmt.Print(cfg.MustString("portal.url"))
// OUT: 
// http://localhost:8080/Portal 
```

## 可用选项

```go
type Options struct {
    // 设置为只读模式. default False
    Readonly bool
    // 解析 ENV 变量名称. default True
    ParseEnv bool
    // 解析变量引用 "%(varName)s". default False
    ParseVar bool

    // 变量左侧字符. default "%("
    VarOpen string
    // 变量右侧字符. default ")s"
    VarClose string

    // 忽略键名称大小写. default False
    IgnoreCase bool
    // 默认的section名称. default "__default"
    DefSection string
    // 路径分隔符，当通过key获取子级值时. default ".", 例如 "section.subKey"
    SectionSep string
}
```

- 应用选项

```go
cfg := ini.New()
cfg.WithOptions(ini.ParseEnv,ini.ParseVar, func (opts *Options) {
    opts.SectionSep = ":"
    opts.DefSection = "default"
})
```

## 参考 

- [go-ini/ini](https://github.com/go-ini/ini) ini parser and config manage
- [dombenson/go-ini](https://github.com/dombenson/go-ini) ini parser and config manage

## License

**MIT**
