## 乐观锁和悲观锁

* 悲观锁：相信并发是绝大部分的，并且每一个线程都必须要达到目的的。

* 乐观锁：相信并发是极少数的，假设运气不好遇到了，就放弃并返回信息告诉它再次尝试。因为它是极少数发生的


## 数据库并发问题

假如两个线程同时修改数据库同一条记录，就会导致后一条记录覆盖前一条，从而引发一些问题，常见的就是卖东西减少库存

例如总共10件商品，卖完就没有了，多个线程同时买入，商品数据减1
```sql
CREATE TABLE temp(
Id int identity(1,1) primary key not null,
Amount int not null
)

insert into temp(Amount) values(100)
```

不加锁的代码

```sql
declare @count as int

begin tran
    select @count=Amount from temp
    WAITFOR DELAY '00:00:05' --模拟并发，故意延迟5秒
    update temp set Amount=@count-1
commit TRAN
 
SELECT * FROM temp

```


##悲观锁

### 直接加锁

更新加UPDLOCK

悲观锁一定成功，但在并发量特别大的时候会造成很长堵塞甚至超时，仅适合小并发的情况

```sql
declare @count as int

begin tran
    select @count=Amount from temp with(UPDLOCK)
    WAITFOR DELAY '00:00:05' --模拟并发，故意延迟5秒
    update temp set Amount=@count-1
commit TRAN
 
SELECT * FROM temp

```


## 乐观锁

可以解决并发带来的数据错误问题，但不保证每一次调用更新都成功，可能会返回'更新失败'


### 方案
 

表中加上时间戳，每次更新前获取时间戳，更新的时候比较时间戳是否被修改，更新需要同时更新时间戳


```sql
ALTER TABLE temp ADD updateStamp TIMESTAMP NOT null --首先给表加一列timestamp


declare @count as int
DECLARE @flag AS TIMESTAMP
DECLARE @rowCount AS int
begin tran
    select @count=Amount,@flag=updateStamp from temp
    WAITFOR DELAY '00:00:05'
    update temp set Amount=@count-1 WHERE updateStamp=@flag --这里加了条件
    SET @rowcount=@@ROWCOUNT  --获取被修改的行数
commit TRAN
 
--对行数进行判断即可
 
IF @rowCount=1
    PRINT '更新成功'
ELSE
    PRINT '更新失败'
	
```