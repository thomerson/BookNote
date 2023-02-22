## NoSql

```Not Only SQL```/非关系型的数据库



### 关系型的数据库


ACID规则

* atomicity

* consistency

* isolation

* durability

### 非关系型的数据库

1. CAP原理

对于一个分布式计算系统来说，不可能同时满足以下三点:

* 一致性(Consistency) (所有节点在同一时间具有相同的数据)
* 可用性(Availability) (保证每个请求不管成功或者失败都有响应)
* 分区容错性(Partition tolerance) (系统中任意信息的丢失或失败不会影响系统的继续运作)

![CAP](https://www.runoob.com/wp-content/uploads/2013/10/cap-theoram-image.png)


2. BASE

    是NoSQL数据库通常对可用性及一致性的弱要求原则:

* Basically Available --基本可用

* Soft-state --软状态/柔性事务。 "Soft state" 可以理解为"无连接"的, 而 "Hard state" 是"面向连接"的

* Eventually Consistency -- 最终一致性， 也是 ACID 的最终目的。

### NoSql优点

- 高可扩展性
- 分布式计算
- 低成本
- 架构的灵活性，半结构化数据
- 没有复杂的关系

### NoSql缺点

- 没有标准化
- 有限的查询功能（到目前为止）
- 最终一致是不直观的程序

### 常用NoSql

* Mongo

    文档存储

    文档存储一般用类似json的格式存储，存储的内容是文档型的。这样也就有机会对某些字段建立索引，实现关系数据库的某些功能

* [Redis]()

    key-value存储

    可以通过key快速查询到其value