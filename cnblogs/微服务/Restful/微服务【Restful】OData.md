## OData

```Open Data Protocol```

**一种基于REST的数据访问方式**

### 五种设计原则

1. 数据多样性存储

    在一个服务里面可以定义多种数据的存储。

2. 向下兼容

    客户端和服务端可以使用不同版本的OData服务，每个服务都可以向下兼容。

3. REST原则

    遵循REST原则。

4. 容易扩展

    如果需要额外的服务，应该能够进行简单的扩展。

5. 简单

### 实施OData

如果需要实施OData服务，需要完成以下四个部分：

1. OData模型

    定义数据结构，一般发生在后端系统。

2. OData协议

    支持CRUDQ（创建，读取，修改，删除，查询）功能，数据的传输可以使用XML或者JSON。

3. OData客户端库

    保证了客户端能够使用库函数方便的访问OData服务。注意，客户端库并不是必须的，但是尽量有，这样可以节省大量的编码工作。

4. OData服务

    可以最终被客户端访问的服务。

### OData服务的结构

* 服务文档（Service Document）
* 服务元结构文档（Service Metadata Document）

文档内容

* 实体（Entity）
* 实体类型（Entity Type）
* 实体集合（Entity Set）
* 属性（Property）
* 导航属性（Navigation Property）
* 关联（Association）

### OData的操作

* 创建

    * HTTP请求类型： POST

    * 成功返回：201

* 读取（包括单条读取-read_entity,多条读取read_entityset）

    * HTTP请求类型：GET

    * 成功返回：200

* 更新

    * HTTP请求类型：PUT

    * 成功返回：204

* 删除

    * HTTP请求类型：DELETE

    * 成功返回：204

* 查询

    * HTTP请求类型：GET/POST

    * 成功返回：200/201

