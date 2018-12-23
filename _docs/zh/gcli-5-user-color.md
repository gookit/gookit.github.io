---
title: 使用颜色输出
permalink: /docs/zh/gcli-use-color
key:
---

## 颜色输出展示

![colored-demo]({{ site.rawghUrl }}/_examples/images/color-demo.jpg)

## 如何使用

```go
package main

import (
    "github.com/gookit/color"
)

func main() {
    // simple usage
    color.Cyan.Printf("Simple to use %s\n", "color")

    // internal theme/style:
    color.Info.Tips("message")
    color.Info.Prompt("message")
    color.Info.Println("message")
    color.Warn.Println("message")
    color.Error.Println("message")
    
    // custom color
    color.New(color.FgWhite, color.BgBlack).Println("custom color style")

    // can also:
    color.Style{color.FgCyan, color.OpBold}.Println("custom color style")
    
    // use defined color tag
    color.Print("use color tag: <suc>he</><comment>llo</>, <cyan>wel</><red>come</>\n")

    // use custom color tag
    color.Print("custom color tag: <fg=yellow;bg=black;op=underscore;>hello, welcome</>\n")

    // set a style tag
    color.Tag("info").Println("info style text")

    // prompt message
    color.Info.Prompt("prompt style message")
    color.Warn.Prompt("prompt style message")

    // tips message
    color.Info.Tips("tips style message")
    color.Warn.Tips("tips style message")
}
```

## 构建风格

```go
// 仅设置前景色
color.FgCyan.Printf("Simple to use %s\n", "color")
// 仅设置背景色
color.BgRed.Printf("Simple to use %s\n", "color")

// 完全自定义 前景色 背景色 选项
style := color.New(color.FgWhite, color.BgBlack, color.OpBold)
style.Println("custom color style")

// can also:
color.Style{color.FgCyan, color.OpBold}.Println("custom color style")
```

## 使用内置风格

### 基础颜色

> 支持在windows `cmd.exe` 使用

- `color.Bold`
- `color.Black`
- `color.White`
- `color.Gray`
- `color.Red`
- `color.Green`
- `color.Yellow`
- `color.Blue`
- `color.Magenta`
- `color.Cyan`

```go
color.Bold.Println("bold message")
color.Yellow.Println("yellow message")
```

### 扩展风格主题 

> 支持在windows `cmd.exe` 使用

- `color.Info`
- `color.Note`
- `color.Light`
- `color.Error`
- `color.Danger`
- `color.Notice`
- `color.Success`
- `color.Comment`
- `color.Primary`
- `color.Warning`
- `color.Question`
- `color.Secondary`

```go
color.Info.Println("Info message")
color.Success.Println("Success message")
```

### 使用颜色html标签

> **不** 支持在windows `cmd.exe` 使用，但不影响使用，会自动去除颜色标签

使用颜色标签可以非常方便简单的构建自己需要的任何格式

```go
// 使用内置的 color tag
color.Print("<suc>he</><comment>llo</>, <cyan>wel</><red>come</>")
color.Println("<suc>hello</>")
color.Println("<error>hello</>")
color.Println("<warning>hello</>")

// 自定义颜色属性
color.Print("<fg=yellow;bg=black;op=underscore;>hello, welcome</>\n")
```

> **更多关于颜色库的使用请访问 [gookit/color](https://github.com/gookit/color)**
