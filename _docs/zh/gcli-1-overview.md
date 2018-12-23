---
title: Go 命令应用工具库
permalink: /docs/zh/gcli
key:
---

[![GoDoc](https://godoc.org/github.com/gookit/gcli?status.svg)](https://godoc.org/github.com/gookit/gcli)
[![Go Report Card](https://goreportcard.com/badge/github.com/gookit/gcli)](https://goreportcard.com/report/github.com/gookit/gcli)
<iframe src="https://ghbtns.com/github-btn.html?user=gookit&repo=gcli&type=star&count=true" frameborder="0" scrolling="0" width="170px" height="20px"></iframe>

Go的命令行应用，工具库，运行CLI命令，支持命令行色彩，用户交互，进度显示，数据格式化显示，生成bash/zsh命令补全脚本

## 截图展示

![app-help]({{ site.rawghUrl }}/_examples/images/app-help.jpg)

## 功能特色

- 使用简单方便，轻量级，无额外依赖
- 支持添加多个命令，并且支持给命令添加别名
- 输入的命令错误时，将会提示相似命令（包含别名提示）
- 快速方便的添加选项绑定 `--long`，支持添加短选项 `-s`
- 支持绑定参数到指定名称, 支持必须`required`，可选，数组`isArray` 三种设定
  - 运行命令时将会自动检测，并按对应关系收集参数
- 支持丰富的颜色渲染输出, 由[gookit/color](https://github.com/gookit/color)提供
  - 同时支持html标签式的颜色渲染，兼容Windows
  - 内置`info,error,success,danger`等多种风格，可直接使用
- 内置提供用户交互方法: `ReadLine`, `Confirm`, `Select`, `MultiSelect` 等
- 内置提供进度显示方法: `Txt`, `Bar`, `Loading`, `RoundTrip`, `DynamicText` 等
- 自动根据命令生成帮助信息，并且支持颜色显示
- 支持为当前CLI应用生成 `zsh`,`bash` 下的命令补全脚本文件
- 支持将单个命令当做独立应用运行

## GoDoc

- [godoc for gopkg](https://godoc.org/gopkg.in/gookit/cliapp.v2)
- [godoc for github](https://godoc.org/github.com/gookit/gcli)

## 快速开始

如下，引入当前包就可以快速的编写cli应用了

```bash
import "github.com/gookit/gcli"
```

```go 
package main

import (
    "runtime"
    "github.com/gookit/gcli"
    "github.com/gookit/gcli/_examples/cmd"
)

// for test run: go run ./_examples/cliapp.go && ./cliapp
func main() {
    runtime.GOMAXPROCS(runtime.NumCPU())

    app := cliapp.NewApp()
    app.Version = "1.0.3"
    app.Description = "this is my cli application"
    // app.SetVerbose(gcli.VerbDebug)

    app.Add(cmd.ExampleCommand())
    app.Add(&gcli.Command{
        Name: "demo",
        // allow color tag and {$cmd} will be replace to 'demo'
        UseFor: "this is a description <info>message</> for command", 
        Aliases: []string{"dm"},
        Func: func (cmd *cliapp.Command, args []string) error {
            gcli.Println("hello, in the demo command")
            return nil
        },
    })

    // .... add more ...

    app.Run()
}
```

## 参考项目

- `issue9/term` https://github.com/issue9/term
- `beego/bee` https://github.com/beego/bee
- `inhere/console` https://github/inhere/php-console
- [ANSI转义序列](https://zh.wikipedia.org/wiki/ANSI转义序列)
- [Standard ANSI color map](https://conemu.github.io/en/AnsiEscapeCodes.html#Standard_ANSI_color_map)
- go package: `golang.org/x/crypto/ssh/terminal`

## License

**[MIT]({{ githubUrl }}/gookit/gcli/blob/master/LICENSE)**

