---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Golang 工程师"
subtitle: ""
summary: "牛客网Golang工程师"
authors: []
tags: ["Go"]
categories: ["interview","Go"]
date: 2020-10-04T21:21:50+08:00
lastmod: 2020-10-04T21:21:50+08:00
featured: false
draft: false
toc: true

# Featured image
# To use, add an image named `featured.jpg/png` to your page's folder.
# Focal points: Smart, Center, TopLeft, Top, TopRight, Left, Right, BottomLeft, Bottom, BottomRight.
image:
  caption: ""
  focal_point: ""
  preview_only: false

# Projects (optional).
#   Associate this post with one or more of your projects.
#   Simply enter your project's folder or file name without extension.
#   E.g. `projects = ["internal-project"]` references `content/project/deep-learning/index.md`.
#   Otherwise, set `projects = []`.
projects: []
---

# go

### 问了两个goroutine如何控制运⾏ 

答:有数据流动就⽤channel,如果只是并发环境下保证数据安全就⽤sync.Mutex

### 问了GoMirco

简单说了说⽹关,限流 

### \3. ⼀道很简单的Go题⽬，Go怎么做深拷⻉。

### \4. Map是线程安全的吗？怎么解决并发安全问题？

### \5. sync.Map 怎么解决线程安全问题？看过源码吗？

### \6. copy是操作符还是内置函数

### \7. slice的底层原理

### \8. map的底层原理

\11. Go⾥⾯⼀个协程能保证绑定在⼀个内核线程上⾯的。。

\13. defer的执⾏顺序  defer panic("") 

Go切⽚和数据之间的区别 

切⽚怎么扩容 

扩容过程中需不需要重新写⼊ 



Go的协程可以不可以⾃⼰让出cpu

Go的协程可以只挂在⼀个线程上⾯吗 

⼀个协程挂起换⼊另外⼀个协程是什么过程？



defer 其实想问和recorver的配合

slice 分配在堆上 还是栈上

GMP并发模型 goroutine切换的时候上下⽂环境放在哪⾥ 

说⼀下reflect



golang中channel调⽤问题，切⽚和 

数组区别和底层



Slice、map都是安全的吗 



golang的gmp



go语句基础：

defer recover panic 执⾏顺序2020/10/3 

go多线程 



Go 协程简单⽤法；

Go func与method之前的那个Receiver是什么？（答：类似Java的实例本⾝，效果同java中的this关键字，同时在go method也可以把这个

Receiver当做参数来正常使⽤）；

Go的闭包语法，答了内部函数对外层函数局部变量的引⽤，类似的还有js、java的lambda；





channel 在哪些场景下使⽤会 panic？（关闭 / 写⼊⼀个已经关闭的 channel）



什么情况下 M 会进⼊⾃旋的状态？（M 是系统线程。为了保证⾃⼰不被释放，所以⾃旋。这样⼀旦有 G 需要处理，M 可以直接使⽤，不 

需要再创建。M ⾃旋表⽰此时没有 G 需要处理）





go ⾥的 syncLock 和 channel 的性能有区别吗？





Golang 怎么在并发编程中等待多个 goroutine 结束



Golang 内存分配的实现

Golang slice 不断 append，是如何给它分配内存的？slice 如果分配的 capacity 是 10，那当容量到多⼤的时候才会扩容？8、9、10？



对于学习 Golang 原理可以参考 Draven ⼤⼤的电⼦书：《Go 语⾔设计与实现》，期待出实体





Go 并发优秀在哪⾥ 

⾯试官：不要只在概念上说性能问题，需要通过实际的测试，benchmark等说明 



go gmp 调度

go 垃圾回收，什么时候触发



分布式缓存实现

go 常⻅数据结构

go map结构实现，并发安全否



golang 协程机制 

协程的栈空间⼤⼩有限制吗？会主动扩展吗？

golang context 应⽤场景

context 的数据结构（树）

golang 的 waitGroup ⽤法

golang 性能问题怎么排查？（profile）





 go语⾔协程调度模型

\7. go语⾔GC

\8. go语⾔的性能的优劣





panic，recover怎么⽤？

5.如果⼀个协程panic了，整个程序会怎么样？

6.Golang垃圾回收。

7.Channel怎么实现的？

8.怎么控制多个协程：定时开始，定时退出，条件开始，条件退出。（现场写）

9.Golang调度机制。



11.⽹络IO的时候会出现什么情况？

12.内存分配。

13.Mutex

14.任务队列怎么实现2020/10/3 

GO 

localhost:8888 

5/37

15.怎么控制并发量

16.怎么阻塞⼀个协程。

17.select怎么⽤。

18.什么时候会触发GC。

19.GC怎么调优，有哪些调优⽅法。



golang中两个map对象如何⽐较。

3.golang多态、⽗类⽅法重写



5.golangGC

6.golang怎么操作内核线程





\2. Golang基础

\- Go中struct组合与Java继承的区别

\- defer关键字使⽤

\- channel有缓冲、⽆缓冲区别

\- GPM模型以及原理（Goroutine、Processor、 Machine）





golang的MPG模型，goroutine和线程的区别

goroutine的调度是出现在什么情况下，调度时做了什么 

问interface,说了底层实现 

问channel,说了底层实现 

问如果有⼤量请求如何处理，我说消息队列，线程池，异步io，引导我说多路复⽤ 

问select,poll和epoll



5.silce遇到过哪些坑，原理？append可能导致底层数组改变

6.slice作为函数参数怎么解决上⾯的问题？答return返回，⾯试官说可以传slice指针

7.channel实现原理，为什么不⽤加锁？

8.goroutine的理解？讲了下MPG模型



go GMP（源码级分析）

go map slice 实现（源码分析以及slice内存泄漏分析）





go 内存逃逸分析（分析了栈帧，讲五种例⼦，描述堆栈优缺点，点头） 

是否有逃逸分析过（没）

defer recover 的问题（⾃⼰了解不多，简单介绍）



go 内存分配

go 协程怎么切换的

go 同步，channel的实现 



(2)go waitgroup 的坑



定时器

go相关知识点（内存分配、go优缺点、go错误处理有什么优缺点） 



9，golang的defer，channel，reflect，多线程panic recover（不会），interface，使⽤interface有什么好处（？？）



4.go 逃逸分析（完全没听过）



3.Go GPM模型

Go 管理依赖 gomod命令，gomod最后的版本号如果没有tag，是怎么⽣成的



数组和切⽚的区别 协程同步的⽅式 waitgroup和context区别 如何处理异常 defer



介绍下go的chan,chan可以做什么 

你们的项⽬如何实现限流器，请⽤chan实现⼀种限流器，也可以不⽤chan实现 

线程进程协程区别

go协程好处

gmp模型



go的new make区别

go的slice和array区别

pprof定位问题与调优 







平时⽤的⼯具链和技术栈是什么 golang 踩过坑吗? 

答了之前 PingCAP ⾯试时⾯试官问的 for-range ⾥的 go-routine 闭包捕获问题 

这段 golang 代码有没有 bug（还是⼀个 for-range 的坑) 

有 bug，for-range 的 value 引⽤拷⻉问题



go 中 sync.map

答： cas + dirty map + read map



golang的基础问题，⽐如包管理，⽐如值传递，⽐如协程。 

问了⼀些Golang的基本知识，如slice⽤copy和左值进⾏初始化的区别；

channel是否线程安全等；



第⼀个是golang除了goroutine还有什么处理并发的⽅法；

golang 的管道怎么⽤；我说是channel。







问：给定n个并发量，并发处理数组

​    答：使⽤channel实现。 



对golang的测试问的很久，性能调优⽅⾯的；



问等待所有goroutine结束，怎么做？ 

答使⽤sync.WaitGroup。

map的底层实现。



问：怎么⽤go实现⼀个栈。 

答：其实就是⼀个切⽚的问题，注意pop的时候⼀定要注意切⽚⻓度检查，否则会panic。 

问：有个考察defer和panic执⾏顺序的问题。 

答：defer是先注册后调⽤(最后注册的最先被执⾏)。所有defer都执⾏完后，才会显⽰panic⾥⾯的东西。



golang 题⽬，panic执⾏顺序，a1b2c3交替打印输出，⽤goroutine实现。 



1.go的调度

2.go struct能不能⽐较？

3.go defer（for defer）

4.select可以⽤于什么？

5.context包的⽤途？



18.当go服务部署到线上了，发现有内存泄露，该怎么处理



7.主协程如何等其余协程完再操作



11.实现消息队列（多⽣产者，多消费者）



10.go-micro 微服务架构怎么实现⽔平部署的，代码怎么实现？ 

11.micro怎么⽤

12.怎么做服务发现的





1、go init 的执⾏顺序，注意是不按导⼊规则的（这⾥是编译时按⽂件名的顺序执⾏的）2020/10/3 

GO 

localhost:8888 

13/40 

2、interface nil ⽐较。 

3、原⽣map⾮线程安全，加锁以及sync.Map{}的实现。 

4、channel no buffer以及buffer的区别。 

5、如何删除slice中间的元素（s = append(s[:i],s[i+1,]...),我感觉其实就是切⽚的应⽤。 

6、怎么保存在程序崩溃时的数据，当时没理解到，我觉得是（defer+reciver） 

7、go实现⼀个SQL Pool（可以借鉴database/sql pool的实现） 

8、go 怎么控制查询timeout （context） 





5、go的oop与传统的oop的区别。 





Context 包的作⽤ 

编译时做接⼝检查（PS: E.g. var _ InterfaceName := (*TypeName)(nil)） 

运⾏时做接⼝检查（PS: E.g. _, ok := TypeName.(InterfaceName)）

Go 反射机制

Go 如何实现继承和多态

Go 结构体内嵌后的命名冲突 



go map实现

go map是并发安全的吗，不是，怎么实现并发安全

sync.map讲⼀下, sync.Map是如何实现的， sync.Map锁的粒度如何 



9、init函数能被外部调⽤吗？



go channel说⼀下，channel被关闭还可以读吗



3.讲⼀下defer

defer有什么特性 

假设在⼀个函数体中对临界资源进⾏加锁和解锁，使⽤defer进⾏解锁和⾃⼰⼿动解锁有什么区别？2020/10/3 



3.⽤defer进⾏recovery在runtime层⾯是什么样的？ 这个我不知道 没答出来..…

4.反射了解吗？



6.slice底层，append底层什么的。

7.从切⽚中取切⽚，底层会变化吗，什么时候会变化？回答了扩容，⾯试官说还有，没答上来，最后也忘了问。



13.Go如何调度，假设4核的cpu应该有⼏个线程或者说有⼏个M，那能有⼏个groutinue，groutinue数量的上限是多少？



12.Go⼀般怎么取map

13.如果⼀个map没申请空间，去向⾥⾯取值，会发⽣什么情况。我记得好像是返回默认值，⾯试官问我确定吗…



25.groutinue什么时候会被挂起



\7. context上下⽂控制。

\8. channel 怎么实现线程安全



\11. 如何实现⼀个每次遍历顺序都⼀样的map。



6.检测Golang内存泄漏的⼯具

7.Golang协程池

8.Golang context包的功能



12.Golang如何优雅的结束协程。如何做到⼀个模块最多分配50个协程，⼀个模块最多只分配10个协程这种。



在协程的GMP模型中，如果⼀个io请求很⼤的话，这个时候P要怎么处理呢。

如何限制 goroutine 并发数⽬：channel 或 WaitGroup



实现链表反序，⾃定义数据结构



