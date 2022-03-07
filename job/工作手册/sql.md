## 说明

在使用db过程中的一些总结
db默认使用的都是sql server


## 查询

* 禁止在where条件中对表的列进行操作
```sql
select * from t where t.createStamp+'' = ''  --错误写法，举个例子
```
* 使用sp_execute代替exec执行字符串sql
	* sp_execute可以缓存执行计划
	* 防止sql注入
* 参数化传参，禁止向sql中拼入参数
	* 防止sql注入


