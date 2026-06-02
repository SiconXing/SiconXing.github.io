---
title: "Go 并发编程：goroutine 与 channel 实战"
date: 2026-05-25 10:00:00 +0800
categories: [Go, 编程]
tags: [Go, 并发, goroutine, channel, 后端]
pin: true
math: false
image:
  path: /assets/img/avatars/avatar.svg
  alt: Go
---

## 前言

Go 语言的并发模型是其最大的亮点之一。通过 goroutine 和 channel，你可以轻松写出高效且安全的并发程序。

## goroutine：轻量级线程

```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("Worker %d 开始\n", id)
    time.Sleep(time.Second)
    fmt.Printf("Worker %d 完成\n", id)
}

func main() {
    var wg sync.WaitGroup

    for i := 1; i <= 5; i++ {
        wg.Add(1)
        go worker(i, &wg)  // 启动 goroutine
    }

    wg.Wait()  // 等待所有 worker 完成
    fmt.Println("所有 worker 已完成")
}
```

## Channel：goroutine 间的通信管道

### 无缓冲 channel

```go
func main() {
    ch := make(chan string)  // 无缓冲 channel

    go func() {
        ch <- "Hello from goroutine"  // 发送（阻塞直到有接收者）
    }()

    msg := <-ch  // 接收（阻塞直到有发送者）
    fmt.Println(msg)
}
```

### 有缓冲 channel

```go
func main() {
    ch := make(chan int, 3)  // 缓冲大小为 3

    // 不阻塞，缓冲区未满
    ch <- 1
    ch <- 2
    ch <- 3

    fmt.Println(<-ch)  // 1
    fmt.Println(<-ch)  // 2
    fmt.Println(<-ch)  // 3
}
```

## 实战：并发爬虫

```go
package main

import (
    "fmt"
    "sync"
)

type Fetcher interface {
    Fetch(url string) (body string, urls []string, err error)
}

// Crawl 递归爬取网页
func Crawl(url string, depth int, fetcher Fetcher) {
    type entry struct {
        urls  []string
        depth int
    }

    worklist := make(chan entry)
    var n int  // 等待发送的活跃 goroutine 数

    // 启动第一个任务
    n++
    go func() {
        _, urls, _ := fetcher.Fetch(url)
        worklist <- entry{urls, 0}
    }()

    seen := make(map[string]bool)
    for ; n > 0; n-- {
        e := <-worklist
        if e.depth >= depth {
            continue
        }
        for _, u := range e.urls {
            if !seen[u] {
                seen[u] = true
                n++
                go func(url string) {
                    _, urls, _ := fetcher.Fetch(url)
                    worklist <- entry{urls, e.depth + 1}
                }(u)
            }
        }
    }
}
```

## `select` 多路复用

```go
func main() {
    ch1 := make(chan string)
    ch2 := make(chan string)

    go func() {
        time.Sleep(1 * time.Second)
        ch1 <- "来自 ch1"
    }()

    go func() {
        time.Sleep(2 * time.Second)
        ch2 <- "来自 ch2"
    }()

    for i := 0; i < 2; i++ {
        select {
        case msg := <-ch1:
            fmt.Println("收到:", msg)
        case msg := <-ch2:
            fmt.Println("收到:", msg)
        case <-time.After(3 * time.Second):
            fmt.Println("超时!")
            return
        }
    }
}
```

## 总结

- **goroutine** 是 Go 的并发基础，启动成本极低
- **channel** 实现 goroutine 间安全的数据传递
- **`sync.WaitGroup`** 用于等待一组 goroutine 完成
- **`select`** 实现多路复用和超时控制

> 记住 Go 的并发哲学：**不要通过共享内存来通信，而要通过通信来共享内存。**
