## 碎片

当对索引所在的基础数据表进行增删改时，若存储的数据进行了不适当的跨页（SQL Server中存储的最小单位是页，页是不可再分的），就会导致索引碎片的产生。

* 外部碎片

    插入的数据使页与页之间造成断续，比如，插入的数据正好在页中最后一行，被挤出到别的页的数据，与原来的页之间没有了连续，这后果就严重了，这种情况就是外部的碎片。

* 内部碎片

    当索引页没有用到最大量时就产生了内部碎片。


## 碎片处理

0. 查看表空间碎片化的一些统计信息 ```dbcc showcontig```


```sql
use ${数据库名}
dbcc showcontig with all_indexes 
--查看指定表的所有索引的碎片信息
dbcc showcontig (${表名}) with all_indexes   
--查看指定表、指定索引的碎片信息
dbcc showcontig (${表名},${索引名})
```

统计脚本

```sql
select 
　　 db_name() as dbname,
    t.name as tablename,
    s.name as schemaname,
    p.rows as rowcounts,
    sum(a.total_pages) * 8 as totalspacekb, 
    cast(round(((sum(a.total_pages) * 8) / 1024.00), 2) as numeric(36, 2)) as 总共占用空间mb,
    sum(a.used_pages) * 8 as 总使用空间kb, 
    cast(round(((sum(a.used_pages) * 8) / 1024.00), 2) as numeric(36, 2)) as 总使用空间mb, 
    (sum(a.total_pages) - sum(a.used_pages)) * 8 as 碎片化空间kb,
    cast(round(((sum(a.total_pages) - sum(a.used_pages)) * 8) / 1024.00, 2) as numeric(36, 2)) as 碎片化空间mb
from 
    sys.tables t
inner join      
    sys.indexes i on t.object_id = i.object_id
inner join 
    sys.partitions p on i.object_id = p.object_id and i.index_id = p.index_id
inner join 
    sys.allocation_units a on p.partition_id = a.container_id
left outer join 
    sys.schemas s on t.schema_id = s.schema_id
where 
    t.is_ms_shipped = 0
    and i.object_id > 0
group by 
    t.name, s.name, p.rows
order by 
    总共占用空间mb desc
```

1. 删除索引并重建

2. 使用DROP_EXISTING语句重建索引

3. 使用ALTER INDEX REBUILD重新生成索引。(推荐)

4. 使用ALTER INDEX REORGANIZE重新组织索引。(推荐)

![重建索引](https://img2023.cnblogs.com/blog/999484/202301/999484-20230108211527239-1994335328.png)

### REBUILD和Reorganize区别

Rebuild 是重新创建，将Index之前占用的空间释放，重新申请空间来创建index

Reorganize 是重新组织，将index的叶子节点进行重新组织

