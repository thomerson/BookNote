## InfluxDB

1. 时序数据库

    主键永远是时间time


2. 高效的数据模型，称为Tag-Value模型

    用标签(Tag)来表示数据的元数据，而字段(Field)则表示实际的数值数据。这种数据模型非常适合处理大量的时间序列数据，并且可以轻松地进行数据的聚合和查询。

3. 保存策略

    influxdb 是没有update的（一般数据仓库只用户数据存储与查询），是保存策略来设置数据的保存时间

### 概念

1. database

    对应数据库

2. measurement

    对应数据库中的表

    * series 数据序列

3. points

    对应表中的一行数据

    * time 时间戳

    * field 数据

    * tags 标签
 


4. bucket




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

Telegraf, InfluxDB, Chronograf, Kapacitor

1. Telegraf **[数据采集工具](http://www.baidu.com/link?url=fmoVMThqUBFNLEcn_LNTOPofB6jD9VITu3EJoDdBWr-BGGlYlgfqD2rgi56rONkBQvYwz0ombHtekRFII9ndtK)**
2. InfluxDB
3. Chronograf 网页管理，可视化
4. Kapacitor 数据监控

InfluxDB的开源版本在单个节点上运行