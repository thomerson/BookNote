## 启动模板

* app: 应用程序模板.

* module: 模块/服务模板.

* console: 控制台模板.

* WPF: WPF模板.

* MAUI: MAUI模板.

0. 安装abp cli

```shell
dotnet tool install -g Volo.Abp.Cli
```

1. 新建解决方案

```shell
# 默认模板是app
abp new Acme.BookStore -t app

# 指定UI框架

abp new Acme.BookStore -u angular

```

参数说明

* -t/--template
    template 模板

* -u/--ui 
    UI框架

    * mvc

    * angular

* -d/--database-provider

    指定数据库连接

    * ef

    * mongodb
    

* -m/--mobile

    指定移动应用程序框架

    * react-native

