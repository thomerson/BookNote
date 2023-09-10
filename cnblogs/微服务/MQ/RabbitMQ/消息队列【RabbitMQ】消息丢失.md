## 消息丢失

https://zhuanlan.zhihu.com/p/425264198

![消息传递](https://pic1.zhimg.com/80/v2-248eebcbe7cf0675e0d9cf1ae2a447c0_720w.webp)


生产者发送丢失
RabbitMQ 中提供了 publisher confirm 机制来避免消息发送到 MQ 的过程中丢失的问题。消息发送到 MQ 以后，会返回一个确认结果给生产者，用于表示消息是否确认成功。该确认结果存在两种请求：

publisher-confirm
该类型是 发送者确认 ，存在两种情况

消息成功投递到交换机，返回 ack
消息未投递到交换机，返回 nack
publisher-return
该类型是 发送者回执 ，存在两种情况

消息投递到交换机，且成功分发到队列，返回 ack
消息投递到交换机，但未成功分发到队列，返回 nack


开启发送确认，这里可以支持两种类型

simple：同步等待 confirm 结果，直到超时
correlated：异步回调，定义 ConfirmCallback，MQ返回结果时会回调这个 ConfirmCallback



1. 生产者确认机制

2. 持久化

    交换机持久化
    
    队列持久化
    
    消息持久化

3. 消费者确认机制

    manual (手动确认)
    
    auto (自动确认)
    
    98none (关闭 ack)