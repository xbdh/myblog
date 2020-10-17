---
# Documentation: https://sourcethemes.com/academic/docs/managing-content/

title: "MySQL"
subtitle: ""
summary: "MySQL 操作"
authors: []
tags: ["MySQL"]
categories: ["interview","MySQL"]
date: 2020-10-03T20:32:21+08:00
lastmod: 2020-10-03T20:32:21+08:00
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

## mysql

### 问了MySQL的存储过程, 

<br>

### Mysql怎么去查询的，什么时候⾛索引，什么时候不⾛ 

<br>

### MySQL的引擎,主要讲了默认引擎改成Innodb,还有Innodb和Myisam的区别

<br>

### MySQL left join、inner join：inner的就是差集、left的就是保左边；

<br>

MySQL的隔离级别，

<br>

MySQL索引相关的⾯试题，搜这个的时候，也许会碰到MyISAM、InnoDB、B/B+树、磁盘IO块与系统⻚、主索引与辅 

助索引、m叉树的分裂与合并……之类的，这样对MySQL的认知⼜多了⼀些。过些时⽇，你可能想着完整的去学⼀遍，这时候买本《⾼ 

性能MySQL》来看，⽽且你之前学到的隔离级别、索引它⾥⾯都提到了，并且更加全⾯和仔细，从基本概念的出现到最终的常⽤场景， 

都给你列出来了。 

###  b树和b+树的区别，然后具体的索引⽅式与常⻅的⽇志类型

### 1~3范式，概念 

### ACID

，my sql索引，查询优化



\8. b树和b+树的区别，然后具体的索引⽅式与常⻅的⽇志类型



数据库四种隔离级别 

数据库出现读性能问题如何解决 

数据库有哪些索引类型（主键索引，哈希表索引这些）





sql和nosql数据库的区别 

数据库底层的索引是怎么实现的？ 

b树，b+树的区别？ 

什么情况下b树⽐b+树好？





数据库事务，隔离级别 

各个隔离级别怎么实现的，原理

RR能解决幻读吗，为什么

MVCC

数据库中的锁 

乐观锁、悲观锁适⽤于什么情况，并发，读多写少 

分布式锁

zookeeper、mysql、redis 、etcd 怎么实现分布式锁，各有什么优缺点，⽣产中⼀般⽤那个

zookeeper原理，怎么保持⾼可⽤

cap





事务的⼏个特性 

什么是事务的⼀致性



7.联合索引的使⽤原则



\5. mysql的存储引擎

\16. innodb与myisam的区别

\17. innodb的索引有哪些

\18. 事务是什么

\19. 如果⼀个表查询，插⼊等很慢，你怎么做



 mvcc、乐观锁 详细说说怎么实现的 

如果有事务A查询⾏A 事务B修改⾏A并commit 此时事务A再修改⾏A 问此时会发⽣什么（不太会）

\8. B-tree B+tree区别 为啥⽤B+不⽤B 不⽤红⿊ 不⽤av



\10. mysql中innodb引擎 

\11. 事务的特性以及问题



9.mysql怎么存储时间？

10.mysql把邮戳转化为⽇常格式时间的函数？ 



mysql查询速度慢如何优化，如何添加索引(覆盖索引/前缀匹配)。



4.mysql索引重建问题



10.⼤表通过主键ID删数据会不会慢 为什么 



\6. MySQL 两种引擎



\- B+树，为什么不⽤B树⼆叉树

\- 聚簇索引和普通索引的区别



4.sql如何优化（索引，返回需要的字段，避免全表扫描（不进⾏null判断，少进⾏⼤于⼩于like操作，不适⽤or连接，少⽤in））

5.事务如何理解（ACID）



mysql和redis的区别

myisam和innodb的区别

redis的数据类型 

哪个数据类型有⽤跳表实现？

mysql和redis能进⾏分布式存储吗？

mysql怎么做容灾备份？





5.⼿机号⽤mysql中的什么数据类型

6.B+树的叶⼦结点存什么

7.sql慢查询

9.分析 sql语句的时间 



7.新建⼀个索引会发⽣什么

6.mysql建索引的原则



给了⼏个sql语句，判断查询是否能覆盖索引，需要考虑 聚簇索引和联合索引的顺序。

Mysql索引的数据结构和MongoDB的数据结构 



MySQL主从复制 

Mysql 索引 

最终⼀致性（项⽬⽤到） 



mysql 索引慢分析（线上开启slowlog，提取慢查询，然后仔细分析explain 中 tye字段以及extra字段，发⽣的具体场景及mysql是怎么做 

的，被表扬回答的不错）

mysql 分布式id（项⽬⽤到的）



(1)mysql checkpoint





(4)mysql 分库分表平滑扩容⽅案 





数据库中死锁的情况 











4.三范式，反三范式？什么时候反三范式？反三范式有什么坏处

5.联合索引，给定⼀些情况问是否会⽤到索引





三种问题（脏读，不可重复度，幻读的问题）； 

如果不⽤串⾏化，如何解决三种存在的问题（不⽤串⾏化，如何保持⾼的效率，也不会出现问题）；

MySQL有哪些索引，介绍⼀下B+树具体的结构； 

聚集索引和⾮聚集索引； 





MySQL 的存储引擎⽤的是什么?（InnoDB）为什么选 InnoDB?

⼏乎所有公司⽤ MySQL 都⽤ InnoDB，降低踩坑成本；聚簇索引，MVCC

MySQL 的聚簇索引和⾮聚簇索引有什么区别? 

聚簇索引的叶⼦节点是数据节点（⽐如定义了主键时的主键索引），⾮聚簇索引叶⼦节点是指向数据块的指针

B+树和⼆叉树有什么区别和优劣?

B+树是多叉树，深度更⼩，B+树可以对叶⼦节点进⾏顺序遍历，B+树能够更好地利⽤磁盘扇区；⼆叉树：实现简单 

针对⼀个场景设计索引，具体场景忘记了，反正考察的是联合索引与列选择性的知识 

现有⼀个新的查询场景, 要怎么解决? 

假如要查 A in () AND B in (), 怎么建索引? 

只给选择性⾼的⼀列建索引，这⾥因为两个都是范围查询所以另⼀个是⾛不到索引的（这⾥答的不好，其实也可以建联合索引然后⽤ 

（A,B) in ((1,2),(3,4)) 的⽅式去查） 

查 A in () AND B in () 时, MySQL 是怎么利⽤索引的? 

先⾛⼀个⾮聚簇索引，查询出⾏数据后再⽤另⼀列回表做筛选 

假如查询 A in (), MySQL 是针对 N 个值分别查⼀次索引, 还是有更好的操作?







写SQL语句，在A表中不在B表中的数据

14、Limit在单表数据量极⼤的时候分⻚怎么做优化

15、不同系统中的MySQL语句执⾏完结果数据怎么保持⼀致







a、项⽬⽇均访问量 

b、数据存储⽅式 

c、如果磁盘不够，数据如何存储（如，⼀个表数据就超过500G，怎么解决？ 分表分库） 

d、数据备份⽅式，备份如何保证⼀致性



sql的锁；

left join和inner join的区别；



问：百万级Mysql怎么处理。 

答：1-主备 2-读写分离 3-集群 4-负载均衡 5-查询缓存 6-raid



mysql表中⼀亿条个⼈信息，如何按⾝份证号查找⽤⼾。



数据库⽅⾯有mongodb和mysql的区别，出乎意料的还有被互联⽹第⼀次考察sql语句。



.mysql引擎：innodb与myisam的区别。

.有想过使⽤内存存储数据，然后设计策略保证节点之前的数据⼀致性吗。类似做⼀个集群出来，这样会不会⽐直接使⽤redis集群性能 

⾼。（我内⼼：我要是有这⽔平还学什么redis。。。） 



\1. mysql如何恢复drop掉的数据，主从复制不是应⽤在这个场景的（配置mysql集群）。

\2. 整个消息队列服务down掉了（不是仅仅的消费者⽅down掉这种），再重启有什么恢复⼿段吗



⼀对⼀、⼀对多、多对多怎么考虑（分析微博、关注、评论、点赞)主要这⾥卡得⽐较久



使⽤ database/sql 和使⽤ gorm 的区别

## SQL

给出⼀个建表语句：有 id，phone，password，status 这四个字段，其中 id 作为主键，并建⽴了联合索引 (phone, password, status)， 

回答以下⼏个问题： 

如何判断⽤⼾是否登录成功（写出 SQL 语句） 

如何防⽌⽤⼾重复注册⼿机（写出 SQL 语句） 

如何找出已重复注册的⼿机（写出 SQL 语句）2020/10/3 

GO 

localhost:8888 

22/40 

对于 SELECT * FROM user WHERE phone = "xxx" AND password = "***" ⽤到什么索引 

对于 SELECT * FROM user WHERE phone = "xxx" AND status = "***" 索引会失效吗 



4.sql语句怎么改变表的结构

5.sql语句做查询是什么样⼦的

