## 分区表  分区视图

分区表可以从物理上将一个大表分成几个小表，但是从逻辑上来看，还是一个大表。


### 什么时候需要分区表

1. 数据库中某个表中的数据很多。

2. 数据是分段的


### 分区的方式

* 水平分区

    水平表分区就是将一个具有大量数据的表，进行拆分为具有相同表结构的若干个表；


* 垂直分区

    垂直表分区就是把一个拥有多个字段的表，根据需要进行拆分列，然后根据某一个字段进行关联

### 如何创建分区表

1. 创建数据库文件组

可以点击数据库属性在文件组里面添加


```sql
alter database <数据库名> add filegroup <文件组名>

---创建数据库文件组
alter database testSplit add filegroup ByIdGroup1
alter database testSplit add filegroup ByIdGroup2
 
```

2. 创建数据库文件

可以点击数据库属性在文件里面添加

将不同的文件放在文件组中。当然一个文件组中也可以包含多个不同的文件。

如果可以的话，将不同的文件放在不同的硬盘分区里，最好是放在不同的独立硬盘里。要知道IQ的速度往往是影响SQL Server运行速度的重要条件之一。将不同的文件放在不同的硬盘上，可以加快SQL Server的运行速度。

 
```sql
alter database <数据库名称> add file <数据标识> to filegroup <文件组名称>

--<数据标识> （name:文件名，fliename:物理路径文件名，size:文件初始大小kb/mb/gb/tb，filegrowth:文件自动增量kb/mb/gb/tb/%,maxsize:文件可以增加到的最大大小kb/mb/gb/tb/unlimited）

alter database testSplit add file 
(name=N'ById1',filename=N'J:\Work\数据库\data\ById1.ndf',size=5Mb,filegrowth=5mb)
to filegroup ByIdGroup1
alter database testSplit add file 
(name=N'ById2',filename=N'J:\Work\数据库\data\ById2.ndf',size=5Mb,filegrowth=5mb)
to filegroup ByIdGroup2
 
```

3. 创建分区表

 

    1. 创建分区函数

    目的是用来规范不同数据存放到不同目录的标准，简单讲就是如何分区

    分区函数只定义了分区的方法，此方法具体用在哪个表的那一列上，则需要在创建表或索引是指定

        ```sql
        create partition function 分区函数名(<分区列类型>) as range [left/right] 
        for values (每个分区的边界值,....) 

        --创建分区函数
        CREATE PARTITION FUNCTION [bgPartitionFun](int) AS RANGE LEFT FOR VALUES (N'1000000', N'2000000', N'3000000', N'4000000', N'5000000', N'6000000', N'7000000', N'8000000', N'9000000', N'10000000')

        --删除分区

		--删除分区语法
		drop partition function <分区函数名>
		
		--删除分区函数 bgPartitionFun
		drop partition function bgPartitionFun  --只有没有应用到分区方案中的分区函数才能被删除
		```

    2. 创建分区方案

        指定分区对应的文件组

        分区函数必须关联分区方案才能有效，然而分区方案指定的文件组数量必须与分区数量一致，哪怕多个分区存放在一个文件组中。

		```sql
		--创建分区方案语法
		create partition scheme <分区方案名称> as partition <分区函数名称> [all]to (文件组名称,....) 
		
		--创建分区方案,所有分区在一个组里面
		CREATE PARTITION SCHEME [bgPartitionSchema] AS PARTITION [bgPartitionFun] TO ([ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1], [ByIdGroup1])
		
        --删除分区方案

        --删除分区方案语法
        drop partition scheme<分区方案名称>

        --删除分区方案 bgPartitionSchema
        drop partition scheme bgPartitionSchema1
		```
 

    3. 创建分区表

    如果在表中创建主键或唯一索引，则分区依据列必须为该列

		```sql
		--创建分区表语法
		create table <表名> (
		<列定义>
		)on<分区方案名>(分区列名)
		
		--创建分区表
		create table BigOrder (
		OrderId              int                  identity,
		orderNum             varchar(30)          not null,
		OrderStatus          int                  not null default 0,
		OrderPayStatus       int                  not null default 0,
		UserId               varchar(40)          not null,
		CreateDate           datetime             null default getdate(),
		Mark                 nvarchar(300)        null
		)on bgPartitionSchema(OrderId)
		```

 

    4. 创建分区索引

        ```sql
		--创建分区索引语法
		create <索引分类> index <索引名称> 
		on <表名>(列名)
		on <分区方案名>(分区依据列名)
		
		--创建分区索引
		CREATE CLUSTERED INDEX [ClusteredIndex_on_bgPartitionSchema_635342971076448165] ON [dbo].[BigOrder] 
		(
			[OrderId]
		)WITH (SORT_IN_TEMPDB = OFF, IGNORE_DUP_KEY = OFF, DROP_EXISTING = OFF, ONLINE = OFF) ON [bgPartitionSchema]([OrderId])
		
		--使用分区索引查询，可以避免多个cpu操作多个磁盘时产生的冲突。
        ```
 


    5. 查看分区表明细信息

		```sql
		--查看分区依据列的指定值所在的分区 
		--查询分区依据列为10000014的数据在哪个分区上
		select $partition.bgPartitionFun(2000000)  --返回值是2，表示此值存在第2个分区 
		
		
		--查看分区表中，每个非空分区存在的行数
		
		--查看分区表中，每个非空分区存在的行数
		select $partition.bgPartitionFun(orderid) as partitionNum,count(*) as recordCount
		from bigorder
		group by  $partition.bgPartitionFun(orderid)
		
		--查看指定分区中的数据记录 
		---查看指定分区中的数据记录
		select * from bigorder where $partition.bgPartitionFun(orderid)=2
		```

### 分区拆分合并移动

 

1. 拆分分区

    在分区函数中新增一个边界值，即可将一个分区变为2个。

	```sql
	--分区拆分
	alter partition function bgPartitionFun()
	split range(N'1500000')  --将第二个分区拆为2个分区
	```

2. 合并分区

    与拆分分区相反，去除一个边界值即可。

	```sql
	--合并分区
	alter partition function bgPartitionFun()
	merge range(N'1500000')  --将第二第三分区合并
	```
 