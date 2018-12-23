---
title: 编写新的命令
permalink: /docs/zh/gcli-create-command
key:
---

## 编写命令

### 关于参数定义

- 必须的参数不能定义在可选参数之后
- 只允许有一个数组参数（多个值的）
- 数组参数只能定义在最后

### 简单使用

```go
app.Add(&gcli.Command{
    Name: "demo",
    // allow color tag and {$cmd} will be replace to 'demo'
    UseFor: "this is a description <info>message</> for command", 
    Aliases: []string{"dm"},
    Func: func (cmd *gcli.Command, args []string) error {
        gcli.Print("hello, in the demo command\n")
        return nil
    },
})
```

### 使用独立的文件

> the source file at: [example.go]({{ site.githubUrl }}/_examples/cmd/example.go)

```go
package cmd

import (
    "fmt"
    "github.com/gookit/color"
    "github.com/gookit/gcli"
)

// options for the command
var exampleOpts = struct {
    id  int
    c   string
    dir string
    opt string
    names gcli.Strings
}{}

// ExampleCommand command definition
func ExampleCommand() *gcli.Command {
    cmd := &gcli.Command{
        Name:        "example",
        UseFor: "this is a description message",
        Aliases:     []string{"exp", "ex"},
        Func:          exampleExecute,
        // {$binName} {$cmd} is help vars. '{$cmd}' will replace to 'example'
        Examples: `{$binName} {$cmd} --id 12 -c val ag0 ag1
  <cyan>{$fullCmd} --names tom --names john -n c</> test use special option`,
    }

    // bind options
    cmd.IntOpt(&exampleOpts.id, "id", "", 2, "the id option")
    cmd.StrOpt(&exampleOpts.c, "config", "c", "value", "the config option")
    // notice `DIRECTORY` will replace to option value type
    cmd.StrOpt(&exampleOpts.dir, "dir", "d", "", "the `DIRECTORY` option")
    // setting option name and short-option name
    cmd.StrOpt(&exampleOpts.opt, "opt", "o", "", "the option message")
    // setting a special option var, it must implement the flag.Value interface
    cmd.VarOpt(&exampleOpts.names, "names", "n", "the option message")

    // bind args with names
    cmd.AddArg("arg0", "the first argument, is required", true)
    cmd.AddArg("arg1", "the second argument, is required", true)
    cmd.AddArg("arg2", "the optional argument, is optional")
    cmd.AddArg("arrArg", "the array argument, is array", false, true)

    return cmd
}

// command running
// example run:
//  go run ./_examples/cliapp.go ex -c some.txt -d ./dir --id 34 -n tom -n john val0 val1 val2 arrVal0 arrVal1 arrVal2
func exampleExecute(c *gcli.Command, args []string) error {
    fmt.Print("hello, in example command\n")

    magentaln := color.Magenta.Println

    magentaln("All options:")
    fmt.Printf("%+v\n", exampleOpts)
    magentaln("Raw args:")
    fmt.Printf("%v\n", args)

    magentaln("Get arg by name:")
    arr := c.Arg("arrArg")
    fmt.Printf("named array arg '%s', value: %v\n", arr.Name, arr.Value)

    magentaln("All named args:")
    for _, arg := range c.Args() {
        fmt.Printf("named arg '%s': %+v\n", arg.Name, *arg)
    }

    return nil
}
```

- 查看此命令的帮助信息：

```bash
go build ./_examples/cliapp.go && ./cliapp example -h
```

> 漂亮的帮助信息就已经自动生成并展示出来了

![cmd-help]({{ site.rawghUrl }}/_examples/images/cmd-help.jpg)
