
## PostgreSQL

免费的**对象-关系**数据库服务器

```post-gress-Q-L```

## 和MySql等关系型数据库比较

* 数据类型丰富，拓展数据类型，子查询

* 时序数据，地理位置等

    ```时序数据``` 是随时间不断产生的一系列数据，简单来说，就是带时间戳的数据

* 支持NoSql一些特性，key-value,json,xml等

* 开源，拓展性

* 支持多版本并发控制```MVCC```


## 安装

[enterprisedb](https://www.enterprisedb.com/downloads/postgres-postgresql-downloads)


## 命令行工具 SQL Shell(psql)


## 用户界面工具

PgAdmin


## 锁

* 排它锁

```Exclusive Locks```

其他的事务不能对它读取和修改

* 共享锁

```Share Locks```

可以被其他事务读取，但不能修改


<!-- TODO -->
