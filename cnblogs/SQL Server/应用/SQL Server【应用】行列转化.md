## 行列转化


### 行转列

![行转列](https://img2020.cnblogs.com/blog/999484/202103/999484-20210316233741865-986687249.png)

```sql

create table #temp
(
Name nvarchar(10) null,
Course nvarchar(10) null,
Score int null
)

insert into #temp(Name,Course,Score)
select '小李','语文','88' union
select '小李','数学','79' union
select '小李','英语','85' union
select '小明','语文','79' union 
select '小明','数学','89' union
select '小明','英语','87' union
select '小红','语文','84' union
select '小红','数学','76' union
select '小红','英语','92'

select * from #temp

```


1. ```case when```实现

```sql
select Name 姓名,
max(case Course when '语文' then Score end) 语文,
max(case Course when '数学' then Score end) 数学,
max(case Course when '英语' then Score end) 英语,
sum(Score) 课程总分,
cast(avg(Score) as decimal(18,2)) 课程平均分
from #temp
group by Name
```


2. ```pivot```实现转化

```sql
select a.Name 姓名,a.语文,a.数学,a.英语,b.SumScore 课程总分,b.AvgScore 课程平均分
from #temp 
pivot
(
max(Score) -- 指定作为转换的列的值 的列名
for Course -- 指定要转换的列的列名
in(语文,数学,英语) -- 自定义的目标列名，即要转换列的不同的值作为列
)a,
(
select t.Name,sum(t.Score) SumScore,cast(avg(t.Score) as decimal(18,2)) AvgScore
from #temp t
group by t.Name
)b
where a.Name=b.Name
```


### 列转行

![列转行](https://img2020.cnblogs.com/blog/999484/202103/999484-20210316234145845-1636398961.png)

```sql
create table #temp
(
Name nvarchar(10) null,
Chinese int null,
Math int null,
English int null
)

insert into #temp(Name,Chinese,Math,English)
select '小李','88','79','85' union
select '小明','79','89','87' union
select '小红','84','76','92'

select * from #temp
```


1. ```union all```实现

```sql
select t.Name 姓名,t.Course 课程,t.Score 分数 from
(select t.Name,'Chinese' Course,Chinese Score from #temp t
union all
select t.Name,'Math',Math from #temp t
union all
select t.Name,'English',English from #temp t) t
order by t.Name,t.Course
```


2. ```unpivot```实现

```sql
select t.Name 姓名,t.Course 课程,t.Score 分数 
from #temp 
unpivot 
(
Score for Course
in(Chinese,Math,English)
)t
```

