---
title: 立即开始使用
permalink: /docs/zh/gcli-usage
key:
---

先使用本项目下的 [demo](https://github.com/gookit/gcli/_examples/) 示例代码构建一个小的cli demo应用

```bash
% go build ./_examples/cliapp.go                                                           
```

### 打印版本信息

打印我们在创建cli应用时设置的版本信息

```bash
% ./cliapp --version
This is my cli application

Version: 1.0.3                                                           
```

### 应用帮助信息

使用 `./cliapp` 或者 `./cliapp -h` 来显示应用的帮助信息，包含所有的可用命令和一些全局选项

示例：

```bash
./cliapp
./cliapp -h # can also
./cliapp --help # can also
```

### 运行一个命令

```bash
% ./cliapp example -c some.txt -d ./dir --id 34 -n tom -n john val0 val1 val2 arrVal0 arrVal1 arrVal2
```

you can see:

![run_example_cmd]({{ site.rawghUrl }}/_examples/images/run_example_cmd.jpg)

### 显示一个命令的帮助

> by `./cliapp {command} -h` or `./cliapp {command} --help` or `./cliapp help {command}`

![cmd-help]({{ site.rawghUrl }}/_examples/images/cmd-help.jpg)

### 生成命令补全脚本

```go
import  "github.com/gookit/gcli/builtin"

    // ...
    // 添加内置提供的生成命令
    app.Add(builtin.GenAutoCompleteScript())

```

构建并运行生成命令(_生成成功后可以去掉此命令_)：

```bash
% go build ./_examples/cliapp.go && ./cliapp genac -h // 使用帮助
% go build ./_examples/cliapp.go && ./cliapp genac // 开始生成, 你将会看到类似的信息
INFO: 
  {shell:zsh binName:cliapp output:auto-completion.zsh}

Now, will write content to file auto-completion.zsh
Continue? [yes|no](default yes): y

OK, auto-complete file generate successful
```

> 运行后就会在当前目录下生成一个 `auto-completion.{zsh|bash}` 文件， shell 环境名是自动获取的。当然你可以在运行时手动指定

生成的shell script 文件请参看： 

- bash 环境 [auto-completion.bash]({{ site.githubUrl }}/gookit/gcli/blob/master/resource/auto-completion.bash) 
- zsh 环境 [auto-completion.zsh]({{ site.githubUrl }}/gookit/gcli/blob/master/resource/auto-completion.zsh)

预览效果: 

![auto-complete-tips]({{ site.rawghUrl }}/_examples/images/auto-complete-tips.jpg)


