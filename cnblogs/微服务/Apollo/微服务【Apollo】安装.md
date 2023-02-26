## server安装

[下载](https://github.com/apolloconfig/apollo/releases)

需要安装的

* ```apollo-adminservice```

    提供配置的**修改、发布**等功能

* ```apollo-configservice```

    提供配置的**读取、推送**等功能

* ```apollo-portal```

    提供Web界面供用户管理配置


Apollo默认会启动3个服务,分别使用**8070**,**8080**,**8090**端口


### 数据库

MySql

* ApolloPotalDB

    [脚本](https://github.com/apolloconfig/apollo/blob/v1.3.0/scripts/db/migration/portaldb/V1.0.0__initialization.sql)

* ApolloConfigDB

    [脚本](https://github.com/apolloconfig/apollo/blob/v1.3.0/scripts/db/migration/configdb/V1.0.0__initialization.sql)

    需要在每个环境部署一套


### windows安装

java环境

在apollo目录下执行命令

1. 启动apollo-configservice

    可通过-Dsever.port=8080修改默认端口


```shell
java -Xms256m -Xmx256m -Dspring.datasource.url=jdbc:mysql://localhost:3306/ApolloConfigDB?characterEncoding=utf8 -Dspring.datasource.username=root -Dspring.datasource.password= -jar apollo-configservice-2.1.0.jar
```


2. 启动apollo-adminservice

    可通过-Dsever.port=8090修改默认端口

```shell
java -Xms256m -Xmx256m -Dspring.datasource.url=jdbc:mysql://localhost:3306/ApolloConfigDB?characterEncoding=utf8 -Dspring.datasource.username=root -Dspring.datasource.password= -jar apollo-adminservice-2.1.0.jar
```

3. 启动apollo-portal

可通过-Dsever.port=8070修改默认端口

```shell
java -Xms256m -Xmx256m -Dspring.datasource.url=jdbc:mysql://localhost:3306/ApolloPortalDB?characterEncoding=utf8 -Dspring.datasource.username=root -Dspring.datasource.password= -jar apollo-portal-2.1.0.jar
```

4. 打开apollo/admin

### docker安装


<!-- TODO -->

<!-- https://blog.csdn.net/qq_42427109/article/details/120509409 -->
<!-- https://zhuanlan.zhihu.com/p/372889924 -->