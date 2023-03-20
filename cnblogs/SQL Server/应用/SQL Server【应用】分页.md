## 分页

1. top分页

```sql
declare @pageIndex int = 1
declare @pageSize int = 10

select top (@pageSize) *  from [dbo].[AbpUsers] 
where id not in (select top (@pageSize*(@pageIndex-1)) id from [dbo].[AbpUsers] )


```


2. row_number分页


```sql
declare @pageIndex int = 1
declare @pageSize int = 10


select * from
(select *,row_number() over(order by id ) as num from [dbo].[AbpUsers]) as temp
where num between @pageSize*(@pageIndex-1)+1 and @pageSize*@pageIndex

```

3. offset fetch分页

2012版本及以上才支持offset fetch分页


```sql
declare @pageIndex int = 1
declare @pageSize int = 10

SELECT *  
FROM [dbo].[AbpUsers]
ORDER BY id ASC
    OFFSET @pageSize*(@pageIndex-1) ROWS
    FETCH NEXT @pageSize ROWS ONLY
```

