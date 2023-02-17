## 限流

```Rate-Limit```

这个应该后面版本添加的，之前的技术介绍上都没有，看git上最新的api有限流的使用方法


防止请求过多把程序搞宕机了，也可以有效防止爬虫和ddos攻击，预估出服务的处理能力，然后设置限流，可以限制单位时间内的访问量（失败一部分请求比整个服务挂掉强）。


```c#

// Allow up to 20 executions per second.
Policy.RateLimit(20, TimeSpan.FromSeconds(1));

// Allow up to 20 executions per second with a burst of 10 executions.
Policy.RateLimit(20, TimeSpan.FromSeconds(1), 10);

// Allow up to 20 executions per second, with a delegate to return the
// retry-after value to use if the rate limit is exceeded.
Policy.RateLimit(20, TimeSpan.FromSeconds(1), (retryAfter, context) =>
{
    return retryAfter.Add(TimeSpan.FromSeconds(2));
});

// Allow up to 20 executions per second with a burst of 10 executions,
// with a delegate to return the retry-after value to use if the rate
// limit is exceeded.
Policy.RateLimit(20, TimeSpan.FromSeconds(1), 10, (retryAfter, context) =>
{
    return retryAfter.Add(TimeSpan.FromSeconds(2));
});
```
