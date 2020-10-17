---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "操作系统"
subtitle: ""
summary: "OS"
authors: []
tags: ["OS"]
categories: ["interview","OS"]
date: 2020-10-03T20:25:13+08:00
lastmod: 2020-10-03T20:25:13+08:00
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



### 进程，线程，协程的区别

<br>

### 内核态线程和用户态线程的区别

如何切换



### 线程的上下文切换

为什么切换会消耗性能。



### 进程用户态转到内核态的方式



### 上层协程结束了，如果通知到子协程也结束



### 进程和线程切换的时候有什么区别？ 



### 进程的五种状态及状态间的转移

<br>

### 僵尸进程和孤儿进程分别是什么，危害，解决



### 为什么线程消耗比协程大，具体体现在哪些方面 



### 生产中哪些服务用的进程、线程，为什么要这么做，有什么好处

（今天第⼆次问到）nginx master-worker进程、进程与redis 进程、线程 



### 线程共享和不共享的是什么



### 给子进程发⼀个kill信号，夫进程能收到吗



### 进程间通信的几种方式。

你认为哪种方式效率最高？



### 为什么要有共享内存？共享内存在操作系统的那个区域？

（共享内存）

9.使⽤共享内存的通信⽅式如何加锁。

### 进程间通信 匿名管道和命名管道的区别

24.有缓存的管道和没有缓存的管道的区别

### 线程的状态 sleep前后会怎么样



### linux下怎么实现⼀个单例进程



### 协程如果像你说的这么牛逼，为什么只有Go⽀持呢，其他语言为什么这⼀两年才开始有协程库？协程这个概念好多年前就有了不是 

从cpu的⻆度回答，这个问题当时确实问得我有点懵，腾讯总监问的，应该从多核发展的⻆度去考虑，另外结合线程/协程的调度特点）

### 起⼏个线程死循环 cpu会爆吗？



进程挂了共享内存是否还存在，为什么？（不知道，结束之后百度：进程间通信使⽤的数据结构:管道、socket、共享内存、消息队列、 

信号量等，是属于内核级的，⼀旦创建后就由内核管理，若进程不对其主动释放，那么这些变量会⼀直存在，除⾮重启系统。）

mmap了解吗？（不⼤了解，答的将⽂件映射到虚拟内存，减少读写io） 



### 内存管理 



###  内存泄漏 ，内存溢出

区别，危害，解决



### 分页和分段内存管理

区别 ，.内存分⻚的⽬的是什么？

### 内存的缺页中断、页面置换算法



### 虚拟内存

为什么，有什么作用，虚拟内存和物理内存的关系。



### 操作系统中有哪些锁，对应的乐观锁、悲观锁，怎么实现的



### 如何控制并发，加什么锁，读写锁还是互斥锁 



### 硬链接和软链接





### 计算机组成是哪五个部分

运算器、控制器、存储器、输⼊设备和输出设备



### ⼆进制的原码反码补码 

 

### 程序的“压栈”“出栈”含义？ 



### 死锁的四个条件，死锁的预防 





### 操作系统如何实现定时器的，

用什么数据结构 







### 



### 程序从加载到运⾏的过程 





### ping这个指令



### mmu有了解吗? 

 讲讲mmu





### 断点调试的原理

### ctrl + c 会发⽣什么 原理

### 

写数据到磁盘，从系统调⽤开始到真正写到磁盘上 中间copy了多少次

### CPU调度算法



### DNS协议的解析过程



### 内存中堆和栈的区别？临时变量放哪？



### 如何从网卡驱动读取数据 



### io多路复用

16.阻塞是啥？send recv 阻塞⾮阻塞的区别？阻塞的线程占⽤CPU吗？为什么vim⼀个超⼤的⽂件CPU会卡？



18.epoll和select？epoll 边缘触发。



15.IO多路复⽤，同步阻塞，⾮同步阻塞（在进⾏IO操作时，线程只能拿等待IO操作就绪不能进⾏别的操作；⾮同步可以进⾏别的操作并 

且循环确认是否就绪）

16.同步和异步（IO请求时，是否是顺序执⾏的，⾮顺序的是异步，⼀边请求⼀边⼲别的；同步就是还是要等上⼀个做完才能做下⼀个）

17.IO多路复⽤，select，poll和epoll

. Io模型

\13. io多路复⽤的类别

\14. epoll的底层实现





io模型，复⽤模型的区别； 



mmap操作原理 

答： 1.内存映射 2.逻辑/物理地址转换 3. 程序访问触发缺⻚中断 4. 调⻚ 

追问：mmap的问题？ 

答了内存过⼤时会出现频繁的⻚⾯置换 影响效率

直接io与mmap区别？