## 消息队列两种模型

* 推模式/阻塞

  业务实时性高

* 拉模式/非阻塞

  业务实时性低


## 订阅sub与发布pub

* 1对多关系


### 应用场景

1. 互联网场景
    1. 分布式系统
        * 异步
        * 监控
        * 统一配置更新

2. 物联网场景
    1. 手机控制空调
    2. 手机控制汽车开关


### 4要素

* 订阅者
* 发布者
* 主题（事件）
* 主题消息

```c#
using StackExchange.Redis;

namespace DemoRedis
{
    /// <summary>
    /// redis发布订阅
    /// </summary>
    internal class RedisPubSub
    {
        private ConnectionMultiplexer ConnectionMultiplexer;
        public RedisPubSub()
        {
            ConnectionMultiplexer = ConnectionMultiplexer.Connect("127.0.0.1:6379");
        }

        /// <summary>
        /// 订阅
        /// </summary>
        public void Sub(string messageTopic, Action<ChannelMessage> action)
        {
            // 订阅器
            var subscriber = ConnectionMultiplexer.GetSubscriber();

            // 订阅消息
            var messageQueue = subscriber.Subscribe(messageTopic);

            //// 处理消息
            //messageQueue.OnMessage((message) =>
            //{
            //    // 消息内容
            //    var messageContent = message.Message;


            //});

            messageQueue.OnMessage(action);

        }

        /// <summary>
        /// 发布者
        /// </summary>
        public void Pub(string messageTopic, string message)
        {
            // 订阅器
            var subscriber = ConnectionMultiplexer.GetSubscriber();

            // 发布消息
            subscriber.Publish(messageTopic, message);
        }
    }
}


```

### redis发布订阅实现原理

链表

socket通信

### 缺陷

* 消息发送失败就失败了

  解决方案：持久化和重试






