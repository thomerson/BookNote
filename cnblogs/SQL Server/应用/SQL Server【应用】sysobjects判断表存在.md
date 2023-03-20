## sysobjects

在数据库内创建的每个对象（约束、默认值、日志、规则、存储过程等）在sysobjects表中占一行

```sql
SELECT * FROM sysobjects WHERE xtype = 'V'  --查看视图

--判断数据库中是否已经存在某个表，有的话就删除该表
--方法一：
if exists (select * from dbo.sysobjects where id = object_id(N'[dbo].[表名]') and OBJECTPROPERTY(id, N'IsUserTable') = 1)
drop table [dbo].[表名]

--方法二：
if exists (select * from sysobjects where id = object_id(N'表名') and OBJECTPROPERTY(id, N'IsUserTable') = 1)
drop table [dbo].[表名]

--方法三：***********************
if(Exists(Select * From SysObjects Where xtype='U' And Name='表名')) 
drop table [dbo].[表名]

```