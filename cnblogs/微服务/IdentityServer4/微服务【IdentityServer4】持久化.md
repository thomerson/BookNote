## 持久化

IdentityServer4已经对EF Core有很好的支持与封装


1. 安装nuget包

```powershell
install-package IdentityServer4

# IdentityServer4针对EF进行封装的包，支持使用EF进行数据的持久化
install-package IdentityServer4.EntityFramework
# EntityFrameworkCore.SqlServer
install-package Microsoft.EntityFrameworkCore.SqlServer
# 包管理控制台迁移
install-package Microsoft.EntityFrameworkCore.Tools
# 命令迁移
install-package Microsoft.EntityFrameworkCore.Design
```




2. 注册ids4，将内存模式改为从数据库中读取

```c#

// 将内存模式改为从数据库中读取
var strConn = @"Data Source=(localdb)\MSSQLLocalDB;Initial Catalog=IdentityServer4DB;
        Integrated Security=True;Connect Timeout=30;Encrypt=False;TrustServerCertificate=False;
        ApplicationIntent=ReadWrite;MultiSubnetFailover=False";
var migrationsAssembly = typeof(Startup).GetTypeInfo().Assembly.GetName().Name;

services.AddIdentityServer()
    // 配置ConfigurationDBContext上下文  针对配置数据，比如客户端(Client)、资Y源(Resources)等
    .AddConfigurationStore(options =>
    {
        options.ConfigureDbContext = dbBuilder =>
        {
            dbBuilder.UseSqlServer(strConn, t_builder => t_builder.MigrationsAssembly(migrationsAssembly));
        };
    })
    // 配置PersistedGrantDbContext上下文 针对用户授权操作时的数据和临时数据，比如同意授权的数据、Token等
    .AddOperationalStore(options =>
    {
        options.ConfigureDbContext = dbBuilder =>
        {
            dbBuilder.UseSqlServer(strConn, t_builder => t_builder.MigrationsAssembly(migrationsAssembly));
        };
    })
    ;

```

3. 执行迁移脚本


```powershell
add-migration init -Context ConfigurationDBContext -Output Data/Migrations/IDS4/ConfigrationDb
add-migration init -Context PersistedGrantDbContext -Output Data/Migrations/IDS4/PersistedGrantDbContext
```

<!-- TODO -->
[IdentityServer4之持久化很顺手的事](https://mp.weixin.qq.com/s?__biz=MzU1MzYwMjQ5MQ==&mid=2247484661&idx=1&sn=a8592b1e16f3783235d0f40df285c545&chksm=fbf11821cc86913744285799c281f1c7295f4705b6e0a4197a3a9ca03d8e28fb8eb92e736245&cur_album_id=1614998206428266497&scene=189#wechat_redirect)