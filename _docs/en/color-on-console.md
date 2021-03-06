---
title: Go CLI color library
permalink: /docs/en/color
key:
---

[![GoDoc](https://godoc.org/github.com/gookit/color?status.svg)](https://godoc.org/github.com/gookit/color)
[![Build Status](https://travis-ci.org/gookit/color.svg?branch=master)](https://travis-ci.org/gookit/color)
[![Coverage Status](https://coveralls.io/repos/github/gookit/color/badge.svg?branch=master)](https://coveralls.io/github/gookit/color?branch=master)
[![Go Report Card](https://goreportcard.com/badge/github.com/gookit/color)](https://goreportcard.com/report/github.com/gookit/color)

A command-line color library with true color support, universal API methods and Windows support.

Basic color preview:

![basic-color]({{ site.rawghUrl }}/_examples/images/basic-color.png)

## Features

- Simple to use, zero dependencies
- Supports rich color output: 16-color, 256-color, true color (24-bit)
  - 16-color output is the most commonly used and most widely supported, working on any Windows version
  - See [this gist](https://gist.github.com/XVilka/8346728) for information on true color support
- Generic API methods: `Print`, `Printf`, `Println`, `Sprint`, `Sprintf`
- Supports HTML tag-style color rendering, such as `<green>message</>`
- Basic colors: `Bold`, `Black`, `White`, `Gray`, `Red`, `Green`, `Yellow`, `Blue`, `Magenta`, `Cyan`
- Additional styles: `Info`, `Note`, `Light`, `Error`, `Danger`, `Notice`, `Success`, `Comment`, `Primary`, `Warning`, `Question`, `Secondary`

## GoDoc

- [godoc for gopkg](https://godoc.org/gopkg.in/gookit/color.v1)
- [godoc for github](https://godoc.org/github.com/gookit/color)

## Quick start

```bash
import "gopkg.in/gookit/color.v1" // is recommended
// or
import "github.com/gookit/color"
```

```go
package main

import (
    "fmt"
    "github.com/gookit/color"
)

func main() {
    // simple usage
    color.Cyan.Printf("Simple to use %s\n", "color")

    // use like func
    red := color.FgRed.Render
    green := color.FgGreen.Render
    fmt.Printf("%s line %s library\n", red("Command"), green("color"))

    // custom color
    color.New(color.FgWhite, color.BgBlack).Println("custom color style")

    // can also:
    color.Style{color.FgCyan, color.OpBold}.Println("custom color style")

    // internal theme/style:
    color.Info.Tips("message")
    color.Info.Prompt("message")
    color.Info.Println("message")
    color.Warn.Println("message")
    color.Error.Println("message")

    // use style tag
    color.Print("<suc>he</><comment>llo</>, <cyan>wel</><red>come</>\n")

    // apply a style tag
    color.Tag("info").Println("info style text")

    // prompt message
    color.Info.Prompt("prompt style message")
    color.Warn.Prompt("prompt style message")

    // tips message
    color.Info.Tips("tips style message")
    color.Warn.Tips("tips style message")
}
```

Run demo: `go run ./_examples/demo.go`

![colored-out]({{ site.rawghUrl }}/_examples/images/color-demo.jpg)

## Detailed usage

### Basic color

Supported on any Windows version.

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

Run demo: `go run ./_examples/basiccolor.go`

![basic-color]({{ site.rawghUrl }}/_examples/images/basic-color.png)

### Additional styles

Supported on any Windows version.

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
// print message
color.Info.Print("Info message")
color.Success.Print("Success message")

// prompt message
color.Info.Prompt("prompt style message")
color.Warn.Prompt("prompt style message")

// tips message
color.Info.Tips("tips style message")
color.Warn.Tips("tips style message")
```

Run demo: `go run ./_examples/theme_style.go`

![theme-style]({{ site.rawghUrl }}/_examples/images/theme-style.jpg)

### HTML-like tag usage

Not supported on Windows (tags will be stripped).

```go
// use style tag
color.Print("<suc>he</><comment>llo</>, <cyan>wel</><red>come</>")
color.Println("<suc>hello</>")
color.Println("<error>hello</>")
color.Println("<warning>hello</>")

// custom color attributes
color.Print("<fg=yellow;bg=black;op=underscore;>hello, welcome</>\n")
```

- `color.Tag`

```go
// set a style tag
color.Tag("info").Print("info style text")
color.Tag("info").Printf("%s style text", "info")
color.Tag("info").Println("info style text")
```

Run demo: `go run ./_examples/colortag.go`

![color-tags]({{ site.rawghUrl }}/_examples/images/color-tags.jpg)

## 256-color usage

### Set the foreground or background color

- `color.C256(val uint8, isBg ...bool) Color256`

```go
c := color.C256(132) // fg color
c.Println("message")
c.Printf("format %s", "message")

c := color.C256(132, true) // bg color
c.Println("message")
c.Printf("format %s", "message")
```

### Use a 256-color style

Can be used to set foreground and background colors at the same time.

- `color.S256(fgAndBg ...uint8) *Style256`

```go
s := color.S256(32, 203)
s.Println("message")
s.Printf("format %s", "message")
```

Run demo: `go run ./_examples/color256.go`

![color-tags]({{ site.rawghUrl }}/_examples/images/256-color.jpg)

## Use RGB color

### Set the foreground or background color

- `color.RGB(r, g, b uint8, isBg ...bool) RGBColor`

```go
c := color.RGB(30,144,255) // fg color
c.Println("message")
c.Printf("format %s", "message")

c := color.RGB(30,144,255, true) // bg color
c.Println("message")
c.Printf("format %s", "message")
```

 Create a style from an hexadecimal color string:

- `color.HEX(hex string, isBg ...bool) RGBColor`

```go
c := HEX("ccc") // can also: "cccccc" "#cccccc"
c.Println("message")
c.Printf("format %s", "message")

c = HEX("aabbcc", true) // as bg color
c.Println("message")
c.Printf("format %s", "message")
```

### Use a RGB color style

Can be used to set the foreground and background colors at the same time.

- `color.NewRGBStyle(fg RGBColor, bg ...RGBColor) *RGBStyle`

```go
s := NewRGBStyle(RGB(20, 144, 234), RGB(234, 78, 23))
s.Println("message")
s.Printf("format %s", "message")
```

 Create a style from an hexadecimal color string:

- `color.HEXStyle(fg string, bg ...string) *RGBStyle`

```go
s := HEXStyle("11aa23", "eee")
s.Println("message")
s.Printf("format %s", "message")
```

## See also

- [`issue9/term`](https://github.com/issue9/term)
- [`beego/bee`](https://github.com/beego/bee)
- [`inhere/console`](https://github.com/inhere/php-console)
- [ANSI escape code](https://en.wikipedia.org/wiki/ANSI_escape_code)

## License

[MIT](/LICENSE)
