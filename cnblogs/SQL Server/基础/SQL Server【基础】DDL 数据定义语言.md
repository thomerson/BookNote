## DDL

操作数据库，schema，表等语句

Create，Alter，Drop，DECLARE

 

1. database

```sql

--1、说明：创建数据库

Create DATABASE database-name thus


--2、说明：删除数据库

drop database dbname
```
 

2. schema

```sql

--1、创建schema

create schema myschema
```
 

3. table index view

```sql

--1、说明：创建新表

create table tabname(col1 type1 [not null] [primary key],col2 type2 [not null],..)

--根据已有的表创建新表： 

create table tab_new like tab_old --(使用旧表创建新表)

create table tab_new as select col1,col2… from tab_old definition only

--2、说明：删除新表

drop table tabname 

--3、说明：增加一个列

Alter table tabname add column col type

--注：列增加后将不能删除。DB2中列加上后数据类型也不能改变，唯一能改变的是增加varchar类型的长度。

--4、说明：添加主键： 

Alter table tabname add primary key(col) 

--说明：删除主键： 

Alter table tabname drop primary key(col) 

--5、说明：创建索引：

create [unique] index idxname on tabname(col….) 

--删除索引：

drop index idxname

--注：索引是不可更改的，想更改必须删除重新建。

--6、说明：创建视图：

create view viewname as select statement 

--删除视图：

drop view viewname
```
 