---
title: 更多CLI工具使用
permalink: /docs/zh/gcli-more-utils
key:
---

## 进度显示
 
- `progress.Bar` 通用的进度条

```text
25/50 [==============>-------------]  50%
```

- `progress.Txt` 文本进度条

```text
Data handling ... ... 50% (25/50)
```

- `progress.LoadBar` 加载中
- `progress.Counter` 计数
- `progress.RoundTrip` 来回滚动的进度条 

```text
[===     ] -> [    === ] -> [ ===    ]
```

- `progress.DynamicText` 动态消息，执行进度到不同的百分比显示不同的消息

示例:

```go
package main

import "time"
import "github.com/gookit/gcli/progress"

func main()  {
    speed := 100
    maxSteps := 110
    p := progress.Bar(maxSteps)
    p.Start()

    for i := 0; i < maxSteps; i++ {
        time.Sleep(time.Duration(speed) * time.Millisecond)
        p.Advance()
    }

    p.Finish()
}
```

> 更多示例和使用请看 [progress_demo.go]({{ site.githubUrl }}/_examples/cmd/progress_demo.go)

run demos:

```bash
go run ./_examples/cliapp.go prog txt
go run ./_examples/cliapp.go prog bar
go run ./_examples/cliapp.go prog roundTrip
```

## 交互方法
   
console interactive methods

- `interact.ReadInput`
- `interact.ReadLine`
- `interact.ReadFirst`
- `interact.Confirm`
- `interact.Select/Choice`
- `interact.MultiSelect/Checkbox`
- `interact.Question/Ask`
- `interact.ReadPassword`

示例:

```go
package main

import "fmt"
import "github.com/gookit/gcli/interact"

func main() {
    username, _ := interact.ReadLine("Your name?")
    password := interact.ReadPassword("Your password?")
    
    ok := interact.Confirm("ensure continue?")
    if !ok {
        // do something...
    }
    
    fmt.Printf("username: %s, password: %s\n", username, password)
}
```

> 更多示例和使用请看 [interact_demo.go]({{ site.githubUrl }}/_examples/cmd/interact_demo.go)

