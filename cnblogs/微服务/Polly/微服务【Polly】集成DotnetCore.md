## 集成DotnetCore

1. 安装nuget包

```shell
install-package Polly
```

定义一个请求失败的方法

```c#
static HttpResponseMessage ExecuteMockRequest()
{
    // 模拟网络请求
    Console.WriteLine("正在执行网络请求...");
    Thread.Sleep(3000);
    // 模拟网络错误
    return new HttpResponseMessage(HttpStatusCode.BadGateway);
}


```

2. 按照不同的故障处理策略处理

* [微服务【Polly】策略重试]()

* [微服务【Polly】策略断路]()

* [微服务【Polly】策略超时]()

* [微服务【Polly】策略隔离]()

* [微服务【Polly】策略回退]()

* [微服务【Polly】策略缓存]()

* [微服务【Polly】策略包]()

* [微服务【Polly】策略限流]()

[源码](https://github.com/thomerson/Demo/tree/main/dotnet6/Demo.PollyConsole)
