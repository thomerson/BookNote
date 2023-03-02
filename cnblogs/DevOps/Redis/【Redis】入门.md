## Redis应用场景

1. 分布式场景
    1. 异步场景 消息队列
    2. 应用解耦
    3. 流量削峰
        通过判断队列长度组织继续向队列中添加请求

2. 日志场景
    1. 优化日志传输

3. 即时通信
    1. 聊天室（点对点）

## 数据类型

* string

* hash

    键值(key=>value)对集合， 是一个 string 类型的 field 和 value 的映射表，hash 特别适合用于**存储对象**

* list

    简单的字符串列表

* set

    （无序）集合是 string 类型的无序集合

    集合是通过哈希表实现的，所以添加，删除，查找的复杂度都是 O(1)

* zset

    有序集合

    不允许重复

    每个元素都会关联一个double类型的分数。redis正是通过分数来为集合中的成员进行从小到大的排序。
    
    zset的成员是唯一的,但分数(score)却可以重复。

## client和server

* redis client


* redis server


## redis常用命令

* ```SETNX```  只有在 key 不存在时设置 key 的值 ⭐

## 事务


## 管道

## 分区


