## 全文搜索

```full-text```

两种搜索方式

* contain

    包含，类似于like '%关键词%'

* freetext

    将一段文字分词以后对每个词进行搜索

### 注意

* 每个数据库可以包含多个全文目录，一个全文目录可以包含多个全文索引，但一个全文索引只能用于构成一个全文目录


* **只能对表创建全文索引，一个数据表只能创建一个全文索引，一个全文索引可以包含多个字段**


* 创建全文索引的表必须要有一个**唯一的非空索引**，并且这个唯一的非空的索引只能是一个字段，不能是组合字段

### 安装

全文搜索是 SQL Server 数据库引擎的一个可选组件, 需要选择安装

已安装了全文搜索组件，可在服务中查看对应的服务是否运行

![full-text](https://img2020.cnblogs.com/blog/366621/202007/366621-20200713212850161-650068645.png)


```sql

-- 查看对应的数据库是否已开启全文搜索
SELECT DATABASEPROPERTY('{DatabaseName}', 'isfulltextenabled');

-- 返回结果是 0,启用
EXEC sp_fulltext_database 'enable';
```


### 创建全文目录

全文目录

```sql
--创建全文目录
CREATE FULLTEXT CATALOG DefaultFullTextCatalog;
```

或者使用SSMS工具操作

![catalog](https://img2020.cnblogs.com/blog/366621/202007/366621-20200713212903825-1686418031.png)


### table创建全文索引

0. 指定语言

    ```sql
    -- 查看支持的语言
    select * from sys.fulltext_languages
    ```

1. 对Shop表的ShopName和ShopAddress两个字段添加简体中文的全文索引


    ```sql
	CREATE FULLTEXT INDEX ON [dbo].[Shop]
	( 
		[ShopName] LANGUAGE 2052,
		[ShopAddress] LANGUAGE 2052
	) 
	KEY INDEX [PK_Shop] ON DefaultFullTextCatalog
	WITH CHANGE_TRACKING AUTO
    ```

2. 使用CONTAINS全文搜索

    ```sql
    --CONTAINS
    SELECT * FROM dbo.Shop WHERE CONTAINS((ShopName, ShopAddress),N'福田')

    --CONTAINSTABLE
    SELECT T0.ShopID,
		T0.ShopName,
		T0.ShopAddress,
		T1.RANK
	FROM dbo.Shop T0
		INNER JOIN CONTAINSTABLE
				(Shop, ShopAddress, N'福田') T1
			ON T0.ShopID = T1.[KEY]
	ORDER BY T1.RANK DESC;
    ```

    CONTAINSTABLE返回的是符合查询条件的表

    CONTAINSTABLE的查询对每一行返回一个相关性排名值 (RANK) 和全文键 (KEY)。RANK用于表示相关性的匹配程度，它的值在0~1000之间，KEY就是主表的ID

2. 使用freetext全文搜索

    **先把要查询的词句先进行分词然后再查询匹配**

    ```sql
    --FREETEXTTABLE
	SELECT T0.ShopID,
		T0.ShopName,
		T0.ShopAddress,
		T1.RANK
	FROM dbo.Shop T0
		INNER JOIN FREETEXTTABLE
				(Shop, (ShopName, ShopAddress), N'福田的邮局') T1
			ON T0.ShopID = T1.[KEY]
	ORDER BY T1.RANK DESC;

    --查看分词结果
    SELECT * FROM sys.dm_fts_parser ('"福田的邮局"', 2052, 0, 0);
    ```