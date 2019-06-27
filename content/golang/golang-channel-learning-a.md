+++
title = "golang channel"
date = 2018-12-03T21:33:00+08:00
lastmod = 2018-12-04T00:07:40+08:00
tags = ["golang", "channel"]
categories = ["golang"]
draft = false
weight = 3001
+++

## goroutine {#goroutine}

首先我们来看线程,在golang里面也叫goroutine

在读这篇文章之前，我们需要了解一下并发与并行。golang的线程是一种并发机制，而不是并行。它们之间的区别大家可以上网搜一下，网上有很多的介绍。

下面我们先来看一个例子吧

```
package main

import (
    "fmt"
)

func main() {
    go fmt.Println("1")
    fmt.Println("2")
}
```

在golang里面，使用go这个关键字，后面再跟上一个函数就可以创建一个线程。后面的这个函数可以是已经写好的函数，也可以是一个匿名函数

```
package main

import (
    "fmt"
)

func main() {
    var i = 3
    go func(a int) {
        fmt.Println(a)
        fmt.Println("1")
    }(i)
    fmt.Println("2")
}
```

上面的代码就创建了一个匿名函数，并且还传入了一个参数i，下面括号里的i是实参，a是形参。

那么上面的代码能按照我们预想的打印1、2、3吗？告诉你们吧，不能，程序只能打印出2。下面我把正确的代码贴出来吧

```
package main

import (
    "fmt"
    "time"
)

func main() {
    var i = 3
    go func(a int) {
        fmt.Println(a)
        fmt.Println("1")
    }(i)
    fmt.Println("2")
    time.Sleep(1 * time.Second)
}

```

我只是在最后加了一行让主线程休眠一秒的代码，程序就会依次打印出2、3、1。

那为什么会这样呢？因为程序会优先执行主线程，主线程执行完成后，程序会立即退出，没有多余的时间去执行子线程。如果在程序的最后让主线程休眠1秒钟，那程序就会有足够的时间去执行子线程。

线程先讲到这里，下面我们来看看通道吧。


## channel {#channel}

通道又叫channel，顾名思义，channel的作用就是在多线程之间传递数据的。


### 创建无缓冲channel {#创建无缓冲channel}

```
chreadandwrite :=make(chan int)

chonlyread := make(<-chan int) //创建只读channel

chonlywrite := make(chan<- int) //创建只写channel
```

下面我们来看一个例子：

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)
    ch <- 1
    go func() {
        <-ch
        fmt.Println("1")
    }()
    fmt.Println("2")
}
```

这段代码执行时会出现一个错误：fatal error: all goroutines are asleep - deadlock!

这个错误的意思是说线程陷入了死锁，程序无法继续往下执行。那么造成这种错误的原因是什么呢？

我们创建了一个无缓冲的channel，然后给这个channel赋值了，程序就是在赋值完成后陷入了死锁。因为我们的channel是无缓冲的，即同步的，赋值完成后来不及读取channel，程序就已经阻塞了。

这里介绍一个非常重要的概念：channel的机制是先进先出，如果你给channel赋值了，那么必须要读取它的值，不然就会造成阻塞，当然这个只对无缓冲的channel有效。对于有缓冲的channel，发送方会一直阻塞直到数据被拷贝到缓冲区；如果缓冲区已满，则发送方只能在接收方取走数据后才能从阻塞状态恢复。

对于上面的例子有两种解决方案：

1、给channel增加缓冲区，然后在程序的最后让主线程休眠一秒，代码如下：

```
package main

import (
    "fmt"
    "time"
)

func main() {
    ch :=make(chan int,1)
    ch <- 1
    go func() {
        v := <-ch
        fmt.Println(v)
    }()
    time.Sleep(1 * time.Second)
    fmt.Println("2")
}

```

这样的话程序就会依次打印出1、2

2、把ch<-1这一行代码放到子线程代码的后面，代码如下：

```
package main

import (
    "fmt"
)

func main() {
    ch := make(chan int)

    go func() {
        v := <-ch
        fmt.Println(v)
    }()
    ch <- 1
    fmt.Println("2")
}
```

这里就不用让主线程休眠了，因为channel在主线程中被赋值后，主线程就会阻塞，直到channel的值在子线程中被取出。

最后我们看一个生产者和消费者的例子：

```
package main

import (
    "fmt"
    "time"
)

func produce(p chan<- int) {
    for i := 0; i < 10; i++ {
        p <- i
        fmt.Println("send:", i)
        time.Sleep(time.Millisecond * 100)
    }
}
func consumer(c <-chan int) {
    for i := 0; i < 10; i++ {
        v := <-c
        fmt.Println("receive:", v)
        time.Sleep(time.Millisecond * 100)
    }
}
func main() {
    ch := make(chan int)
    go produce(ch)
    go consumer(ch)
    time.Sleep(1 * time.Second)
}

```

在这段代码中，因为channel是没有缓冲的，所以当生产者给channel赋值后，生产者这个线程会阻塞，直到消费者线程将channel中的数据取出。消费者第一次将数据取出后，进行下一次循环时，消费者的线程也会阻塞，因为生产者还没有将数据存入，这时程序会去执行生产者的线程。程序就这样在消费者和生产者两个线程间不断切换，直到循环结束。


### 创建带缓冲channel {#创建带缓冲channel}

下面我们再看一个带缓冲的例子：

```
package main

import (

    "fmt"
    "time"
)
func produce(p chan<- int) {
    for i := 0; i < 10; i++ {
        p <- i
        fmt.Println("send:", i)
    }
}
func consumer(c <-chan int) {
    for i := 0; i < 10; i++ {
        v := <-c
        fmt.Println("receive:", v)
    }
}
func main() {
    ch := make(chan int, 10)
    go produce(ch)
    go consumer(ch)
    time.Sleep(1 * time.Second)
}
```

在这个程序中，缓冲区可以存储10个int类型的整数，在执行生产者线程的时候，线程就不会阻塞，一次性将10个整数存入channel，在读取的时候，也是一次性读取。


## Ref {#ref}

-   <https://blog.csdn.net/netdxy/article/details/54564436>
