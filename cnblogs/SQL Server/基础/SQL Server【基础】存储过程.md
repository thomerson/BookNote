## 存储过程

```SET NOCOUNT { ON | OFF }```

当 SET NOCOUNT 为 ON 时，不返回计数（表示受 Transact-SQL 语句影响的行数）。当 SET NOCOUNT 为 OFF 时，返回计数。


```sql
if (exists (select * from sys.objects where name = 'GetUser')) drop proc GetUser   --判断存储过程是否存在，存在则删除然后重建。
go
create proc GetUser  --创建存储过程 GetUser
@Id int --参数
as 
set nocount on;  --不返回计数，提高应用程序性能
begin --开始
    select * from [dbo].[User] where Id=@Id  --执行sql语句
end;--结束


EXEC GetUser 1;
```

