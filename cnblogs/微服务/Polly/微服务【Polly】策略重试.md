## 重试策略

```Retry```

当发生请求异常或网络错误时，就重试 3 次


```c#
Console.WriteLine($"重试策略");

Policy
        // 1. 指定要处理什么异常
        .Handle<HttpRequestException>()
        //    或者指定需要处理什么样的错误返回
        .OrResult<HttpResponseMessage>(r => r.StatusCode == HttpStatusCode.BadGateway)
        // 2. 指定重试次数和重试策略
        .Retry(3, (exception, retryCount, context) =>
        {
            Console.WriteLine($"开始第 {retryCount} 次重试：");
        })
        // 3. 执行具体任务
        .Execute(ExecuteMockRequest);




```

