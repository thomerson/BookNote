## CHARINDEX

找到对应的字符串，则返回该字符串位置，否则返回0。

```sql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )

--expressionToFind ：目标字符串，就是想要找到的字符串，最大长度为8000 。

--expressionToSearch ：用于被查找的字符串。

--start_location：开始查找的位置，为空时默认从第一位开始查找。

select charindex('test','this Test is Test') --6

--增加开始位置
select charindex('test','this Test is Test',7) --14

--大小写敏感
select charindex('test','this Test is Test'COLLATE Latin1_General_CS_AS) --0

--大小写不敏感
select charindex('Test','this Test is Test'COLLATE Latin1_General_CI_AS) --6


```




## PATINDEX

用来判断一个字符串中是否包含另一个字符串，两种的差异在于，前者是全匹配，后者支持模糊匹配

```sql
select PATINDEX('%ter%','interesting data') --3

select PATINDEX('%t_ng%','interesting data') --8
```