## 事务

事务是作为单个逻辑单元执行的一系列操作，它是一个不可分割的工作逻辑单元。它包含了一组数据库操作命令，这组命令要么全部执行，要么全部不执行。

 

### 特性

1. 原子性```Atomicity```

    事务是一个完整的操作， 事务中所有操作命令必须作为一个整体提交或回滚。如果事务中任何操作命令失败，则整个事务将因失败而回滚。

2. 一致性```Consistency```

    当事务完成时，数据都处于一致状态。

3. 隔离性```Isolation```
    
    对数据进行修改的所有并发事务是彼此隔离的，它不以任何方式依赖或影响其他事务。

4. 持久性```Durability```

    事务提交之后，数据是永久性的，不可再回滚。

```sql
begin tran
--sql 语句1
--sql 语句2
--sql 语句3
commit tran
```

### 事务操作
 

1. begin transaction：开始事务。

2. commit transaction：提交事务。

3. rollback transaction：回滚事务。

4. save transaction：事务保存点。即事务回滚时，可以指定回滚到保存点，而不进行全部回滚。
 

### 事务分类

1. 显式事务

    用 begin transaction 明确指定事务的开始，由 commit transaction 提交事务、rollback transaction 回滚事务到事务结束。

2. 隐式事务

    通过设置 set implicit_transactions on 语句，将隐式事务模式设置为打开。当以隐式事务模式操作时，不必使用 begin transaction 开启事务，当一个事务结束后，这个模式会自动启用下一个事务，只需使用 commit transaction 提交事务或 Rollback Transaction 回滚事务即可。

3. 自动提交事务

     这是 SQL Server 的默认模式，它将每条单独的 T-SQL 语句视为一个事务。如果成功执行，则自动提交。如果错误，则自动回滚。

 

### 存储过程事务提交

1. ```xact_abort on/off```  

    on：开启，事务一旦出问题，全部回滚  off：关闭，不检查事务是否发生错误。

```sql
set xact_abort on
begin tran
--sql语句1
--sql语句2
--sql语句3
commit
```
 

2. 每条操作语句后面判断是否回滚。

```sql
begin tran
--sql语句1
if @@error<>0
 begin
   rollback tran
   return --这里除了return跳出，也可以使用goto+标签跳出事务
 end
--sql语句2
if @@error<>0
 begin
   rollback tran
   return
 end
commit tran
 

try  catch

begin tran
 begin try
    --sql语句1
    --sql语句2
    --sql语句3
 end try
 begin catch
    if @@trancount>0
        rollback tran
 end catch
 if @@trancount>0
    commit tran
 

 
```
 

## 事务隔离级别

　SET TRANSACTION ISOLATION LEVEL < READ COMMITTED | READ UNCOMMITTED | REPEATABLE READ | SERIALIZABLE | SNAPSHOT >
对隔离级别的修改只会影响到当前的连接-所以不必担心会影响到其他的用户。其他用户也影响不了你。

1. ```READ COMMITTED```读已提交

    **默认情况**就是这个，通过READ COMMITTED，任何创建的共享锁将在创建它们的语句完成后自动释放。也就是说，如果启动了一个事务，运行了一些语句，然后运行SELECT语句，再运行一些其他的语句，那么当SELECT语句完成的时候，与SELECT语句相关联的锁就会释放 - SQL Server并不会等到事务结束。

　　动作查询(UPDATE、DELETE、INSERT)有点不同。如果事务执行了修改数据的查询，则这些锁将会在事务期间保持有效。

　　通过设置READ COMMITTED这一默认隔离级别，可以确定有足够的数据完整性来防止脏读。然而，仍会发生非重复性读取和幻读。

2. ```READ UNCOMMITTED```读未提交

　　READ UNCOMMITTED是所有隔离级别中最危险的，但是它在速度方面有最好的性能。
　　设置隔离级别为READ UNCOMMITTED将告诉SQL Server不要设置任何锁，也不要事先任何锁。
　　锁既是你的保护者，同时也是你的敌人。锁可以防止数据完整性问题，但是锁也经常妨碍或阻止你访问需要的数据。由于此锁存在脏读的危险，因此此锁只能应用于并非十分精确的环境中。

3. ```REPEATABLE READ```重复读

　　REPEATABLE READ会稍微地将隔离级别升级，并提供一个额外的并发保护层，这不仅能防止脏读，而且能防止非重复性读取。
防止非重复性读取是很大的优势，但是直到事务结束还保持共享锁会阻止用户访问对象，因此会影响效率。推荐使用其他的数据完整性选项，例如CHECK约束，而不是采用这个选择。
　　与REPEATABLE READ隔离级别等价的优化器提示是REPEATABLEREAD(除了一个空格，两者并无不同)。

4. ```SERIALIZABLE```序列化

    SERIALIZABLE是堡垒级的隔离级别。除了丢失更新以外，它防止所有形式的并发问题。甚至能防止幻读。

　　如果设置隔离级别为SERIALIZABLE，就意味着对事物使用的表进行的任何UPDATE、DELETE、INSERT操作绝对不满足该事务中任何语句的WHERE子句的条件。从本质上说，如果用户想执行一些事务感兴趣的事情，那么必须等到该事务完成的时候。

　　SERIALIZABLE隔离级别也可以通过查询中使用SERIALIZABLE或HOLDLOCK优化器提示模拟。再次申明，类似于READ UNCOMMITTED和NOLOCK，前者不需要每次都设置，而后者需要把隔离级别设置回来。

5. ```SNAPSHOT```快照

　　SNAPSHOT是最新的一种隔离级别，非常想READ COMMITTED和READ UNCOMMITTED的组合。要注意的是，SNAPSHOT默认是不可用的-只有为数据库打开了ALLOW_SNAPSHOT_ISOLATION特殊选项时，SNAPSHOT才可用。

　　和READ UNCOMMITED一样，SNAPSHOT并不创建任何锁，也不实现人和所。两者的主要区别是它们识别数据库中不同时段发生的更改。数据库中的更改，不管何时或是否提交，都会被运行READ UNCOMMITTED隔离级别的查询看到。而使用SNAPSHOT，只能看到在SNAPSHOT事务开始之前提交的更改。从SNAPSHOT事务一开始执行，所有查看到的数据就和在时间开始时提交的一样。

 

 

### 隔离级别对应解决的问题


![隔离级别](https://img2023.cnblogs.com/blog/999484/202301/999484-20230106104155584-739254111.png)
 

### 锁与事务隔离级别


![事务隔离级别](https://img2023.cnblogs.com/blog/999484/202301/999484-20230106111723817-2078118547.png)
