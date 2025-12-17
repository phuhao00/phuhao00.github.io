---
layout: post
title: "Go语言入门指南"
date: 2018-04-21
categories: [编程语言]
tags: [Go, Golang, 教程]
---

## Go 语言简介

Go（也称为 Golang）是由 Google 开发的一种静态类型、编译型的编程语言。它具有简洁的语法、高效的并发处理能力和快速的编译速度。

### 为什么选择 Go？

1. **简洁的语法** - 易于学习和使用
2. **高性能** - 接近 C 语言的执行效率
3. **并发编程** - 内置 goroutine 和 channel
4. **快速编译** - 编译速度极快
5. **丰富的标准库** - 开箱即用的强大功能

### 安装 Go

访问 [Go 官网](https://golang.org/) 下载适合你操作系统的安装包。

验证安装：

```bash
go version
```

### Hello World

创建 `hello.go` 文件：

```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")
}
```

运行程序：

```bash
go run hello.go
```

### 基础语法

#### 变量声明

```go
// 方式1：完整声明
var name string = "Phuhao"

// 方式2：类型推断
var age = 25

// 方式3：简短声明（函数内）
city := "Beijing"
```

#### 函数定义

```go
func add(a int, b int) int {
    return a + b
}

// 多返回值
func divide(a, b float64) (float64, error) {
    if b == 0 {
        return 0, fmt.Errorf("division by zero")
    }
    return a / b, nil
}
```

#### 结构体

```go
type Person struct {
    Name string
    Age  int
}

func main() {
    p := Person{
        Name: "Phuhao",
        Age:  25,
    }
    fmt.Printf("%+v\n", p)
}
```

### 并发编程

Go 最强大的特性之一就是并发编程：

```go
package main

import (
    "fmt"
    "time"
)

func sayHello(name string) {
    for i := 0; i < 3; i++ {
        time.Sleep(100 * time.Millisecond)
        fmt.Printf("Hello, %s!\n", name)
    }
}

func main() {
    go sayHello("World")
    go sayHello("Go")

    time.Sleep(1 * time.Second)
}
```

### 学习资源

- [Go 官方文档](https://golang.org/doc/)
- [Go by Example](https://gobyexample.com/)
- [The Go Programming Language](https://www.gopl.io/)

### 总结

Go 语言是一门现代化的编程语言，特别适合开发：

- 网络服务和 API
- 云原生应用
- 命令行工具
- 分布式系统

开始你的 Go 语言学习之旅吧！🚀
