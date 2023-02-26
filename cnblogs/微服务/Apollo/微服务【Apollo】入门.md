## Apollo

携程框架部门研发的分布式配置中心

集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性

* 服务端

    集中化管理应用不同环境、不同集群的配置，配置修改后能够实时推送到应用端，并且具备规范的权限、流程治理等特性

* 客户端

    * Java

    * Net

### 四个维护

Apollo支持4个维度管理Key-Value格式的配置

1. application

    每个应用都需要有唯一的身份标识，可以在代码中配置```app.id```

2. environment

    在 Apollo 中默认提供了四种环境：```env```
    
    * DEV：开发环境

    * FAT：功能测试环境

    * UAT：集成测试环境

    * PRO：生产环境

3. cluster

    一个应用下不同实例的分组

4. namespace

    **一个应用中不同配置的分组**

    有两种权限

    * public（公共的）： public权限的 Namespace，能被任何应用获取。

    * private（私有的）： 只能被所属的应用获取到。一个应用尝试获取其它应用 private 的 Namespace，Apollo 会报 “404” 异常。

### 本地缓存

Apollo客户端会把从服务端获取到的配置在 本地文件系统缓存 一份，用于在遇到服务不可用，或网络不通的时候，依然能从本地恢复配置，不影响应用正常运行。

本地缓存路径默认位于以下路径，所以请确保 ```/opt/data```或```C:\opt\data\```目录存在，且应用**有读写权限**。

    Mac/Linux： /opt/data/{appId}/config-cache
    
    Windows： C:\opt\data{appId}\config-cache

本地配置文件会以下面的文件名格式放置于本地缓存路径下：

    {appId}+{cluster}+{namespace}.properties


## Apollo架构

![Apollo架构](https://img2020.cnblogs.com/blog/1090617/202007/1090617-20200728215539079-1535962969.jpg)


四个主要模块和核心功能

1. ```ConfigService```

    提供配置的**读取、推送**等功能

    Client的后端服务

2. ```Admin Service```

    提供配置的**修改、发布**等功能

    Portal的后端服务

3. ```Client```

    客户端程序，为应用提供配置获取、实时更新

4. ```Portal```

    提供Web界面供用户管理配置


服务发现

* ```Eureka```




