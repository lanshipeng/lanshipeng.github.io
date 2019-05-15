---
title: gorutine和线程区别
categories: 
- go
tags:
- gorutine
comments: true
---

- Go指南: https://tour.golang.org, https://tour.go-zh.org/
- Go圣经: https://gopl.io, https://github.com/golang-china/gopl-zh
- Go进阶: https://github.com/chai2010/advanced-go-programming-book

<!-- more -->

1. goroutine 比线程更轻量级，可以创建十万、百万不用担心资源问题。

2. goroutine 和 chan 搭配使用，实现多线程、高并发 实现起来要方便很多。

3. 线程的栈内存大小一般是固定的2M  gorutine的栈内存可变 初始大小2kb 随着需求可以扩大到1gb

4. 从调度上讲，线程的调度由 OS 的内核完成；线程的切换需要CPU寄存器和内存的数据交换，从而切换不同的线程上下文。 其触发方式为 CPU时钟。而goroutine 的调度 则比较轻量级，由go自身的调度器完成；其只关心当前go程序内协程的调度；触发方式为 go内部的事件，time.sleep,通道阻塞，互斥量操作等
