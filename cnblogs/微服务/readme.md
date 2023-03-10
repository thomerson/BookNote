![微服务](https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg)

## 主要模块

1. 服务注册与发现
    * [Consul]()
    * ZooKeeper
    * etcd
    * Eureka

2. 网关

    * [Ocelot]()

3. RPC

    * [gRPC]()

* 服务监控
* 服务跟踪
* 服务治理
    * 缓存，限流，熔断，链路追踪 

## 高并发策略

1. 应用级缓存方案（过期策略，回收策略，Lru算法）

2. Http缓存方案

3. 多级缓存方案OpenResty

4. 连接池线程池方案

5. 异步并发方案

6. 扩容集群

7. 队列（内存队列，分布式队列）


## 数据一致性

1. 分布式事务

## 高可用

1. 负载均衡和反向代理

2. 隔离方案（集群隔离和机房隔离）

3. 限流方案（功能限流，缓存限流，ip限流，接口限流，商品限流）

4. 降级

5. 超时与重试机制

6. 回滚机制

7. 压测与预案


## 高并发和高可用

* 分层
    * 逻辑分层
    * 物理部署

* 冗余
    * 集群
    * 备份 定期，冷热
    * 灾备

* 分隔（纵向）

* 异步
    * 共享内存
    * 消息队列

* 分布式架构

* 架构安全
    * 身份认证
    * 防机器人程序
    * 通信加密
    * 垃圾信息过滤

* 信息自动化

    * 自动化发布
    * 自动化代码管理
    * 自动化检测
    * 自动化测试

* 架构集群

    * 反向代理



## 服务网格


## sqldependency

<!-- 
TODO

http://www.identityserver.com.cn/Home/Detail/openidConnect

https://github.com/IdentityServer/IdentityServer4/tree/main/samples/Quickstarts

https://identityserver4.readthedocs.io/en/latest/quickstarts/2_interactive_aspnetcore.html#creating-an-mvc-client


https://www.cnblogs.com/stulzq/p/8119928.html

https://cloud.tencent.com/developer/article/1696517


https://www.cnblogs.com/linianhui/p/oidc-in-action-sso.html

https://img2020.cnblogs.com/blog/824291/202004/824291-20200406223103730-527609758.jpg


https://zhaobingwang.blog.csdn.net/article/details/103824965


http://www.identityserver.com.cn/

https://www.cnblogs.com/qtiger/p/14889151.html

https://www.jb51.net/article/183505.htm

https://www.cnblogs.com/fengchao1000/category/1327628.html

https://www.cnblogs.com/stulzq/category/1060023.html -->