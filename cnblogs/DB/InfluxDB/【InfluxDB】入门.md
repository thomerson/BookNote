## 时序数据库

1. 主键永远是时间time

2. 专注于海量时序数据的高性能读、高性能写、高效存储与实时分析等


## InfluxDB

1. ```Go```语言开发的时序数据库


2. 高效的数据模型，称为```Tag-Value```模型

    用标签(Tag)来表示数据的元数据，而字段(Field)则表示实际的数值数据。这种数据模型非常适合处理大量的时间序列数据，并且可以轻松地进行数据的聚合和查询。

    * ```Tag```

    * ```Field```

3. 保存策略

    influxdb 是没有update的（一般数据仓库只用户数据存储与查询），是保存策略来设置数据的保存时间

### 概念

1. database

    数据库，和关系型数据库一样

2. measurement

    对应数据库中的表

    * series 数据序列

3. points

    对应表中的一行数据

    * time 时间戳

    * field 数据

    * tags 标签
 
4. series

    时序数列

5. bucket

6. Shard

    每一个存储策略下会存在许多 shard，每一个 shard 存储一个指定时间段内的数据，并且不重复，例如 7点-8点 的数据落入 shard0 中，8点-9点的数据则落入 shard1 中。每一个 shard 都对应一个底层的 tsm 存储引擎，有独立的 cache、wal、tsm file。




### 保存策略

InfluxDB本身不提供数据的删除操作，因此用来控制数据量的方式就是定义数据保留策略

```powershell
show retention policies on db_name

--------------------------output---------------------------
name    duration    shardGroupDuration    replicaN    default
default    0        168h0m0s        1        true

```

* duration--持续时间，0代表无限制，如duration 1h，即只保留一小时内的数据

* shardGroupDuration时间，检测的时间窗口，默认为7d

* replicaN--全称是REPLICATION，副本个数



### TICK

集成了**采集**、**存储**、**分析**、**可视化**等能力的开源时序中台

![image](https://img2023.cnblogs.com/blog/999484/202308/999484-20230820163541662-1534047298.png)


1. Telegraf **[数据采集工具](http://www.baidu.com/link?url=fmoVMThqUBFNLEcn_LNTOPofB6jD9VITu3EJoDdBWr-BGGlYlgfqD2rgi56rONkBQvYwz0ombHtekRFII9ndtK)**
2. InfluxDB
3. Chronograf 网页管理，可视化
4. Kapacitor 数据监控

InfluxDB的开源版本在单个节点上运行


### TSM

![image](https://img2023.cnblogs.com/blog/999484/202308/999484-20230820172140516-1203939412.png)


1. cache

    是 wal 文件中的数据在内存中的缓存

2. wal

    持久化数据

3. tsm file

    存放数据

4. compactor

    compactor 组件在后台持续运行，每隔 1 秒会检查一次是否有需要**压缩合并**的数据

    *  cache 中的数据大小达到阀值后，进行快照，之后转存到一个新的 tsm 文件中

    * 合并当前的 tsm 文件，将多个小的 tsm 文件合并成一个，使每一个文件尽量达到单个文件的最大大小，减少文件的数量，并且一些数据的删除操作也是在这个时候完成。


### 文件目录

* meta

    存储数据库的一些元数据，meta 目录下有一个 ```meta.db``` 文件

* wal

    存放预写日志文件，以 ```.wal``` 结尾

* data

    存放实际存储的数据文件，以 ```.tsm``` 结尾

    