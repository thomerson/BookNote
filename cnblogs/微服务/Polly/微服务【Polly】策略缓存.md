## 缓存

将数据存入缓存中，后续的响应可以从缓存中获取; 目的就是为了提升性能；


```c#

Console.WriteLine($"缓存策略");


var memoryCache = new MemoryCache(new MemoryCacheOptions());
var memoryCacheProvider = new MemoryCacheProvider(memoryCache);
var cachePolicy = Policy.Cache(memoryCacheProvider, TimeSpan.FromMinutes(5));

var cachePolicy = Policy.Cache(memoryCacheProvider, new AbsoluteTtl(DateTimeOffset.Now.Date.AddDays(1));

// Define a cache policy with sliding expiration: items remain valid for another 5 minutes each time the cache item is used.
var cachePolicy = Policy.Cache(memoryCacheProvider, new SlidingTtl(TimeSpan.FromMinutes(5));

// Define a cache Policy, and catch any cache provider errors for logging.
var cachePolicy = Policy.Cache(memoryCacheProvider, TimeSpan.FromMinutes(5),
(context, key, ex) => {
      Console.WriteLine($"Cache provider, for key {key}, threw exception: {ex}."); // (for example)
  }
);

```

