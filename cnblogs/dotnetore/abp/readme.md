## ABP

```ASP.NET Boilerplate```

一个开源且文档友好的应用程序框架，提供了基于领域驱动设计(DDD)的体系结构模型

支持.NET Framework和.NET Core

[Github](https://github.com/aspnetboilerplate)


### 主要内容

* 依赖注入

    使用```Castle windsor``` 来实现依赖注入，也支持扩展autofac

* Repository仓储模式

    支持Entity Framework、NHibernate、MangoDB、内存数据库等

* 身份验证与授权管理

* 数据有效性验证

* 审计日志记录

* ```Unit Of Work```/工作单元模式

    为应用层和仓储层的方法自动实现**数据库事务**

***

* 异常处理

* 日志记录

    * log4net

* 多语言/本地化支持

* Auto Mapping自动映射

* 动态Web API层

    自动生成服务而不需要WebApi控制器

* 动态JavaScript的AJax代理处理

    自动创建Javascript 的代理层来更方便使用WebApi

***

* 多租户支持

    每个租户的数据自动隔离，业务模块开发者不需要在保存和查询数据时写相应代码

* 软删除支持

* ```EventBus```实现领域事件(Domain Events)

* 领域驱动

## 环境

* 最新版本7需要.Net7+

* 历史版本6需要.Net6+

## 使用abp cli创新abp项目

1. 安装abp cli

```shell
# install
dotnet tool install -g Volo.Abp.Cli 

# 指定版本 6对应.net6 ,7对应.net7
dotnet tool install -g Volo.Abp.Cli --version 6.0

# update
dotnet tool update -g Volo.Abp.Cli
```

2. 生成新项目

```shell
abp new Acme.BookStore

# 控制台程序
abp new Acme.MyConsoleApp -t console

# wpf程序
abp new Acme.MyWpfApp -t wpf


```

3. 修改```appsettings.json```连接字符串


默认使用 ```Entity Framework Core``` 与 ```MS SQL Server```


-dbms参数来选择其他DBMS，例如```-dbms MySQL```

```json
"ConnectionStrings": {
  "Default": "Server=(LocalDb)\MSSQLLocalDB;Database=BookStore;Trusted_Connection=True"
}
```

4. 数据库迁移

Entity Framework Core ```Code First```

带有 .DbMigrator 的控制台程序用于 应用迁移 和 初始化种子数据. 它在开发和生产环境中都很有用.

初始数据添加了用户```admin```，密码```1q2w3E*```

## nuget安装abp

1. 安装nuget包

```powershell
Install-Package Volo.Abp.AspNetCore.Mvc

```

2. 创建ABP模块 



<!-- https://www.cnblogs.com/xhznl/p/13259036.html -->


## 关键

* DDD

* 仓储

    仓储用于操作领域对象（实际就是操作数据库），通常会为每个聚合根或不同的实体创建对应的仓储。ABP也提供了通用的泛型仓储：```IRepository<TEntity, TKey>```，内置了增删改查基本功能，直接注入就可以使用

* 多租户/```multi-tenancy technology```

    多租户是一种软件架构技术，这种架构可以让多个租户共用相同的系统，并且可以确保各租户间数据的隔离性。

* Unit Of Work/工作单元

    为了保证一次业务操作的数据完整性。



