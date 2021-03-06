---
title: "Hooks"
date: 2021-07-30T09:58:50+08:00
summary: "HTTP请求触发执行任务的简单服务器，使用Go实现"
draft: false
toc: true
tags: ['go','hook','docker','tool']
---

 编写一个HTTP Hook简单服务器程序，发送HTTP请求触发执行预定义的任务。

使用Go语言编写，新建文件main.go，编辑如下：

```go
package main

import (
    "bytes"
    "fmt"
    "gopkg.in/yaml.v2"
    "log"
    "net/http"
    "os/exec"
)

type Job struct {
    Name    string
    HookUrl string
    Command []string
}
type T struct {
    Jobs []Job
}

func IndexHandler(writer http.ResponseWriter, request *http.Request) {

    path := request.URL.Path
    println("path", path)

    fmt.Printf("%v", t)

    for _, job := range t.Jobs {
        println(job.HookUrl)
        if job.Name == path {
            println("matched", job.HookUrl)
            c := &Cli{}
            cmd := exec.Command(job.Command[0],  job.Command[1])
            o, _ := (c.Run(cmd))

            writer.Write([]byte(o))

            break
        }
    }

    writer.Write([]byte("no job"))
}

var t *T

func main() {
    data := `
jobs:
  - name: /test
    hookUrl: test
    command: ['wget','http://baidu.com']
  - name: /test2
    hookUrl: /test2
    command: ['touch','test2.txt']
`
    err := yaml.Unmarshal([]byte(data), &t)
    if err != nil {
        log.Fatalf("error: %v", err)
    }
    fmt.Printf("--- t:\n%v\n\n", t)

    for _, job := range t.Jobs {
        fmt.Printf("name:%v\n", job.Name)
        fmt.Printf("hookUrl:%v\n", job.Command)
        http.HandleFunc(job.Name, IndexHandler)

    }

    log.Fatal(http.ListenAndServe("0.0.0.0:8000", nil))
}

type Cli struct {
    OutStr string
    ErrStr string
}

func (c *Cli) Run(cmd *exec.Cmd) (string, string) {
    var stdout, stderr bytes.Buffer
    cmd.Stdout = &stdout
    cmd.Stderr = &stderr
    err := cmd.Run()
    if err != nil {
        log.Printf("cmd.Run() failed with %s\n", err)
    }

    c.OutStr = string(stdout.Bytes())
    c.ErrStr = string(stderr.Bytes())

    return string(stdout.Bytes()), string(stderr.Bytes())
}
```

# 部署

