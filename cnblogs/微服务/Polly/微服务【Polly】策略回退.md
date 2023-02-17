## 回退

```Fallback```

一般情况，当无法避免的错误发生时，我们要有一个合理的返回来代替失败。

```c#

Console.WriteLine($"回退策略");
Policy.Handle<Exception>()
   .Fallback(() =>
   {
       // do something
   })
   .Execute(ExecuteMockRequest);
   
```

