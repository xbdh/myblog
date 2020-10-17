---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "计算机网络"
subtitle: ""
summary: "Computer Network"
authors: []
tags: [“计算机网络”]
categories: ["interview","network"]
date: 2020-10-03T20:41:45+08:00
lastmod: 2020-10-03T20:41:45+08:00
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

## http

### 登录⻚⾯功能的实现 

### HTTP的重定向机制 

### HTTP的keep-alive机制（传输数据什么的，当时太紧张忘了⾯试官说了什么了）

### cookie和seesion的区别（sessionID的存放点）



### HTTP与HTTPs区别，

常见的对称加密与非对称加密有哪些



### htttps工作流程，加密和认证过程



### HTTPS 加密流程 



### http状态码

301,302,500,502



### https中ssl的握手过程、为什么不⼀直用非对称加密

为啥客⼾端不直接⽤服务器端发送过来的公钥加密信息进⾏传递？



### HTTP 请求的报⽂格式



### http1.0，1.1，2.0，3.0  



### session 和 cookie，jwt，

为什么⽤ sessionId 不⽤ uid；cookie和session，session是在哪的呢？sessionid是怎么拿到，怎么查的呢？



### http的请求方法有哪些

get与post的区别，get和post的应⽤场景

请求头 请求体



### 了解http的断点续传吗？

项⽬断点续传的原理，



### http长连接怎么解决沾包问题 thrift怎么解决沾包问题





### 打开⼀个URL到页面展示的过程 

此处我并没有按照⾯经模板去谈，⽽是根据我⾃⼰实际的使⽤讲了很多，如我说了DNS缓存，本地缓存，浏览器缓存，还有清除DNS缓 

存的命令等。⾯试官⽐较满意。



### HTTP属于TCP/IP中哪⼀层（应⽤层）



### URL的格式

越详细越好

http header ⼲啥⽤的？ 



http连环问题 tcp连环问题 ⻓链接短链接

# tcp

### TCP的粘包拆包有了解过吗 

### tcp报⽂结构 



### 为什么四次挥⼿

TCP四次挥⼿，结合CS两端点的TCP栈和上层应⽤的交互来解释四次挥⼿，以及为何需要中间那个FIN-WAIT-2这个过程，最后由被动关 

闭⼀⽅的上层应⽤通过调⽤socket.closed()来结束数据传输，进⼊最终的FIN模式；

tcp四次挥⼿过程。 

最后⼀次挥⼿为什么要等待2MSL

### 三次握⼿的原因，

为什么不是两次 

然后问在四次挥⼿中，如果产⽣⼤量的time_wait的状态，是由于什么原因造成的，通常需要什么样的⽅法解决



### time_wait，closewait 状态存在的原因

tcp timewait堆积可能的原因、怎么解决 

出现⼤量的原因、怎么优化

timewait、closewait出现，怎么处理

IME_WAIT？有什么⽅法可以避免 TIME_WAIT？TIME_WAIT 是主动断开连接的⼀⽅还是被动断开的⼀⽅？



### 有很多sync_recv状态是发⽣了什么



### 流程中各个协议都是运⾏在哪⼀层的？



### TCP如何保持数据传输的可靠性

（应该加ARQ协议的，校验和，拥塞控制，流量控制）

3、TCP是通过什么机制保障可靠性的？（从四个⽅⾯进⾏回答，ACK确认机制、超时重传、滑动窗⼝以及流量控制，深⼊的话要求详细 

讲出流量控制的机制。） 



### TCP 拥塞控制和流量控制

拥塞控制时怎么判断⽹络出现了拥塞 

tcp调优相关参数 

拥塞控制算法、滑动窗⼝、零窗⼝报⽂

如何进⾏流量控制和拥塞控制 



### TCP 如何实现纠错，防止丢失数据和重复



### TCP UDP 的差别



### 滑动窗⼝的实现

（发送⽅接收到接收⽅已确认数据就会滑动，直到最左边不是已确认为⽌。接收⽅接收到连续数据并发送确认开始滑 动）

### tcp访问⼀个主机如果主机端⼝不存在返回什么信息 

我说应该是time out吧



### tcp ip协议





### 在传输视频是使用UDP而不是TCP

为什么快？



6.谈⼀下TCP协议的拥塞控制

7.慢开始是具体是怎么样的？

8.怎么保证不丢包？

9.如果⼀直丢包，怎么办，⼀直重传的话，能重传多少次？ …



### tcp断开连接时保持2mls的弊端

（我答得是优点。。⾯试官提醒说是要答弊端，弊端？难道是占⽤了客⼾端的端⼝吗）



### 路由器ip包进路由器到出路由器哪些变了 数据链路层呢

###  NAT



### traceroute原理









### 以下进⾏了TCP很多运⽤的问题，

TCP中的数据是如何能做到发送HELLO WORLD不会变成WORLD HELLO（有序性？）、TCP数据 

很⼩如果只有⼀个字节会发⽣什么（粘包？粘包的解决=设置边界，设置头部，保持定⻓报⽂）、接收⽅收到失序报⽂如何确定的（我理 

解是有序性，包有序号，发现序号不连续，边⻢上开始发送三个确认（快重传））、TCP⼀对⼀如何变成⼀对多





5.TCP报⽂中的RST标志位的作⽤。

6.socket read返回0是怎么回事



TCP可靠性、黏包（讲到了nagle算法，⾯试官追问具体怎么合并？合并到多⼤，⼤⼩是多少）

使⽤⼀个TCP⻓连接来发送视频或者⼤⽂本⽂件是否合适



# 其他

### ⽹络通信双⽅的流程

（服务端：bind、listen、accept；客⼾端：connect）



描述⼀下向socket传值的流程？ 

⽹络or⽂件系统socket（⽹络）

protocol选择 （Raw / TCP / UDP） 

发送之后发⽣了什么 （放进了buffer） 

哪⾥的buffer （socket描述符⾃带read / recv buffer）

Send buffer满了怎么办？ （阻塞） 

异步通知？（信号signal） 

你会怎么设计操作系统的信号？（直接说了不懂，没有追问）



### socket 如何标识 

域名解析服务时使⽤的传输层协议

### 网络的协议栈为什么要分层 



什么是跨域，跨域的⽅法 

### ping命令的底层原理？



### arp协议

\- nat 介绍

\- gossip 协议

### 计网转发分组的详细过程

5.⽹络不可达如何排查，例如我当前打不开qq.com



ddos的含义，发⽣在三次握⼿的哪个阶段，为什么会ddos



分布式系统 CAP



### ⽹络IO模型有哪些？

（5种⽹络I/O模型，阻塞、⾮阻塞、I/O多路复⽤、信号驱动IO、异步I/O） 

## rpc

1.Rpc协议⼀般如何做序列化？grpc是怎么做的？

2.rpc服务如何处理⾼并发？多线程多进程有什么区别？使⽤场景。

11.grpc的应⽤场景，相⽐于http它的优势在哪⾥。

## websocket