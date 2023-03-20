<!-- ### 更新 update select

### 查找

#### 查找相同code下的最新一条 -->

<!-- TODO -->

## having

HAVING 子句可以让我们筛选分组后的各组数据

```sql
-- 查找总访问量大于 200 的网站
SELECT Websites.name, Websites.url, SUM(access_log.count) AS nums FROM (access_log
INNER JOIN Websites
ON access_log.site_id=Websites.id)
GROUP BY Websites.name
HAVING SUM(access_log.count) > 200;

```

