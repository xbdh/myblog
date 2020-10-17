---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "Linux"
subtitle: ""
summary: "Linux 操作"
authors: []
tags: ["linux"]
categories: ["interview","linux"]
date: 2020-10-03T20:38:55+08:00
lastmod: 2020-10-03T20:38:55+08:00
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

# linux

### 常见的5个Linux中查看资源的命令

top,free,lsof,ps,netstat。可能指的就是这5个吧





### Linux nginx ⽇志⽂件找出现次数最多的 ip（sort、uniq、awk）

### Linux 查看⼀个端⼝的运⾏情况 





### 怎么查看端⼝是否被占用

8.linux查看端⼝占⽤命令

### 怎么杀死进程，有哪些参数？



### ⼀个⽂件中去重后的ip地址数⽬？

awk命令好像



### 如何查看某个进程占⽤的内存⼤⼩？

ps aux | grep xxx



### 怎么判断⼀个进程的状态，⽤什么命令。





### 怎么查看cpu，磁盘io，⽹络io。

Linux磁盘 io较⾼，怎么排查

### 怎么查看⼀个⽂件的⼤⼩。



### grep怎么用



### 怎么看⼀个进程主要是哪个函数在消耗性能？ 





### 怎么定位⼀个300万次中出现⼀次的错误。







### linux中想对⼀个⽂件中内容相同的⾏去重

(即多⾏内容⼀致就只保留⼀⾏)应该输⼊什么命令执⾏。



### Linux的top命令都可以查看哪些内容？

### 统计⽇志⽂件中502的次数



6.tcpdump使⽤，如何使⽤tcpdump抓到某个主机ip的包。如何查看列出详细信息。

7.如何对⼀个进程进⾏性能优化，确定某个进程的性能瓶颈，主要从⽇志分析到top查看进程的瓶颈点，如果是cpu占⽤⾼使⽤pprof等采集 

⼯具，确定热点函数进⾏优化。



### 线上cpu负载过⾼，怎么排查 

cpu特别⾼如何定位

谈到cat和more的时候讲⼀下区别 

### 如果⼀个打开的⽂件⾥⾯有许多ip地址应该怎么过滤出来？

 如何排查⽹络问题、命令

Linux 命令使⽤： 

查看进程：ps/top

查看内存：free

查看磁盘：df/du



linux 命令: 找出⽂件⾥的match的⼀⾏(cat | grep 感觉不太好) 



10.linux从开机到展⽰给⽤⼾登陆的界⾯，这个过程中都发⽣了什么。 

11.linux检测进程端⼝号被占⽤的命令。

12.能解释⼀下类似ps -aux这种命令具体是怎么获取到相应的信息的吗。linux的proc⽂件夹下记录进程信息的⽂件与linux中的普通⽂件有 

什么区别。

8.shell，如何查看⼀个进程打开了哪些⽂件？



如何检查之前的命令是否运⾏成功 (使⽤ Shell 脚本) 

如何检查⽂件系统中是否存在某个⽂件 (使⽤ Shell 脚本)

Linux ⽂件权限⼀共 10 位⻓度，解释每⼀位的含义 



Linux上怎么调试程序 

Linux 程序写⽂件时 rm  能否成功 rm -f呢

rm -f之后 正在写⽂件的程序会怎样 报什么错 



## vim

### vim怎么查询⼀个单词，怎么做匹配。

### vim怎么跳到最前⾯，怎么跳到最后⾯。





## shell

4.shell ：统计今天的⽇志的个数

5.shell：统计某某和

15.时间、ip、访问信息三列，shell统计访问次数最多的10个ip？不会







## 其他

5.⼀个⽂件太⼤应该怎么打开。