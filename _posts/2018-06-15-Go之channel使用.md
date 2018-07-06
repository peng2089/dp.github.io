---
layout: post
title: Go之channel使用
keywords: golang,channel
description: Go之channel使用
tags: [ golang ]
---
# Go之channel使用

## 创建

```golang
ch := make(chan int)
```

## 无缓冲(unbuffered)通道
> 无缓冲通道上的发送操作将会阻塞, 直到另一个goroutine在对应的通道上执行接受操作.

> 使用无缓冲通道进行通信会导致发送和接受同步化, 因此, 无缓冲通道也成为同步通道.


## 信号传递

下面的示例中，在子goroutine中进行一个需要一段时间的操作，主goroutine可以做一些别的事情，然后等待子goroutine完成。接收方会一直阻塞直到有数据到来。如果channel是无缓冲的，发送方会一直阻塞直到接收方将数据取出。如果channel带有缓冲区，发送方会一直阻塞直到数据被拷贝到缓冲区；如果缓冲区已满，则发送方只能在接收方取走数据后才能从阻塞状态恢复。

```golang
package main

import "fmt"

func main(){
    c := make(chan int)

    go func() {
        fmt.Printf("hello world")
        c <- 1
    }()

    <- c
}
```

## 生产真消费者模式

> 作为消息队列






## 注

1. 通道是一个使用make创建的数据结构的引用. 当复制或者作为参数传递到一个函数时, 复制的是引用.
2. 同种类型的通道可以使用==符号进行比较.