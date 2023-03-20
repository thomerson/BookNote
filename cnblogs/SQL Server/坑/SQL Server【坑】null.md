## 摘要

sql中null的判断有自己独特的逻辑，在写脚本需要注意或者设计表时必填项尽量设置为```not null```

## ```null```的```<>```判断


```sql

--比较下面3个脚本的写法
select * from tb where col <> 3 

select * from tb where col <> 3 or col is null

select * from tb where isnull(col,0) <> 3

--不能将null与其他任何东西进行比较，null甚至不等于null

declare @a int = null
declare @b int = null

if @a = @b 
begin print '=' end
else begin print '!=' end


```

