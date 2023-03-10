## NoSQL

```Not Only SQL```/不仅仅是SQL


### BASE

BASE：Basically Available, Soft-state, Eventually Consistent。

CAP理论的核心是：一个分布式系统不可能同时很好的满足一致性，可用性和分区容错性这三个需求，最多只能同时较好的满足两个。

BASE是NoSQL数据库通常对可用性及一致性的弱要求原则:

* Basically Available 

    基本可用

* Soft-state 

    软状态/柔性事务。 "Soft state" 可以理解为"无连接"的, 而 "Hard state" 是"面向连接"的

    是指允许系统中的数据存在中间状态，并认为该中间状态的存在不会影响系统的整体可用性，即允许系统在不同节点的数据副本之间进行数据传输的过程存在延时。

* Eventually Consistency 

    最终一致性， 也是 ACID 的最终目的。


## NoSQL 数据库分类

* 文档存储

    * MongoDB

    文档存储一般用类似json的格式存储，存储的内容是文档型的。这样也就有机会对某些字段建立索引，实现关系数据库的某些功能。

* key-value存储

    * MemcacheDB

    * Redis

    可以通过key快速查询到其value。一般来说，存储不管value的格式，照单全收

