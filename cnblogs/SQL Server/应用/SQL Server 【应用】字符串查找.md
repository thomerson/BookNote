## CHARINDEX

找到对应的字符串，则返回该字符串位置，否则返回0。

```sql
CHARINDEX ( expressionToFind , expressionToSearch [ , start_location ] )

--expressionToFind ：目标字符串，就是想要找到的字符串，最大长度为8000 。

--expressionToSearch ：用于被查找的字符串。

--start_location：开始查找的位置，为空时默认从第一位开始查找。

```


## PATINDEX

用来判断一个字符串中是否包含另一个字符串，两种的差异在于，前者是全匹配，后者支持模糊匹配

```sql
select PATINDEX('%ter%','interesting data') --3

select PATINDEX('%t_ng%','interesting data') --8
```