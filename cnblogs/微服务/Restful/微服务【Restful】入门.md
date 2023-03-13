### rest

```representational state transfer ```/表述性状态转移

```restful``` 一种架构风格

在请求层面，REST 规范可以简单粗暴抽象成以下两个规则

1. 请求 API 的 URL 表示用来定位资源；

    URL 用来定位资源，跟要进行的操作区分开，这就意味这 URL 不该有任何动词

推荐写法

    /api/teams （对应团队列表）
    /api/teams/123 （对应 ID 为 123 的团队）
    /api/teams/123/members （对应 ID 为 123 的团队下的成员列表）
    /api/teams/123/members/456 （对应 ID 为 123 的团队下 ID 未 456 的成员）

2. 请求的 METHOD 表示对这个资源进行的操作；

    * get 读取

    * post 新增

    * put 

        **幂等**

        **更新全部**，在请求的 body 中需要传入修改后的全部资源主体

    * patch

        **非幂等**，多次相同的请求可能会产生不同的结果

        **局部更新**，在 body 中只需要传入需要改动的资源字段

    * delete

3. 分页过滤排序

以 offset 和 limit 参数来进行分页

    GET /api/users?offset=0&limit=20

支持提供关键词进行搜索，以及排序

    GET /api/users?keyword=john&sort=age

支持根据字段进行过滤

    GET /api/users?gender=male

### URL和URI

* URL

    ```Uniform Resource Locator```/统一资源定位符

    互联网上标准资源的地址

* URI

    ```Universal Resource Identifier```/统一资源标志符

    用来标示抽象或物理资源