Index scan

Index seek

物理读
逻辑读
预读

option compile

## 正确设计表结构和索引

## 避免全表扫描

### 导致索引失效的几种写法以及对应的解决方案

* != <> 全局扫描
* like 全文检索
* where 条件中有or，改用union
* in not in 会导致弃用索引的情况 ，改用exists ，连续数值考虑用between and，
* is null/is not null 不会导致弃用索引导致全表扫描
* 禁止在where 子句中的“=”左边进行函数、算术运算或其他表达式运算 例如select id from t where num/2=100

* 禁止 一个表的索引数超过6个：索引固然可 以提高相应的 select 的效率，但同时也降低了 insert 及 update 的效率

* 禁用游标，放到服务端处理

* 存储过程  SET NOCOUNT ON/OFF




## 分表

* 垂直分表：将部分字段分离出来，设计成分表，根据主表的主键关联
* 水平分表：将相同字段表中的记录按照某种Hash算法进行拆分多个分表

## 分区


## 分库/读写分离



## 后端或Web端缓存
减少数据库读取


## 分库分表带来的问题

* 事务一致性
* 跨库关联查询

* 水平切分的表排序，分页问题

* 主键重复
    * GUID 雪花算法

* 数据迁移和扩容


