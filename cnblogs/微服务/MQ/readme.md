 ## MQ

```Message Queue```/消息队列


在消息的传输过程中保存消息的容器

![消息队列](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAyMC83LzE5LzE3MzY3NTNjNDc1M2M2Zjk?x-oss-process=image/format,png)



### 消息队列的作用

* 解耦

	生产者和消费者没有依赖

* 异步

	不需要等待，提升性能

* 削峰

	生产者不直接发请求到消费者处理，只要发消息发送到MQ，消费者分批次处理请求

	![削峰](https://imgconvert.csdnimg.cn/aHR0cHM6Ly91c2VyLWdvbGQtY2RuLnhpdHUuaW8vMjAyMC83LzE5LzE3MzY3YTlkOTAyY2NhNGY?x-oss-process=image/format,png)

### 使用MQ带来的问题

* 系统可用性降低

* 系统复杂度提高

	* 重复消费
	* 消息丢失

* 一致性


### 常用的MQ

* MSMQ

	<!-- TODO -->

* [RabbitMQ](#rabbitmq)
	* 优点
		* 吞吐量到万级
		* 支持多种语言
		* 社区活跃度高
	* 缺点

		* 商业版需要收费，学习成本较高
	* 适用
		* 中小型公司



* RockerMQ
	* 优点
		* 单机吞吐量十万级
		* 消息可以做到 0 丢失
		* 支持 10 亿级别的消息堆积
	* 缺点
		* 支持的客户端语言不多，目前是 java 及 c++
	* 适用
		* 金融互联网
* Kafka

	**大数据**

	* 优点
		* 性能卓越，吞吐量高，单机写入 TPS 约在百万条/秒，时效性 ms 级，可用性非常高

		* 分布式

		* 界面管理
	* 缺点
		* 社区更新较慢
	
	* 适用

		* 大量数据的互联网公司数据收集

		* 日志采集


* activeMQ

	* 缺点
		* 维护越来越少


### RabbitMQ

* [消息队列【RabbitMQ】入门]()

* [消息队列【RabbitMQ】消息模型]()

* [消息队列【RabbitMQ】安装]()

* [消息队列【RabbitMQ】dotnet6记录日志]()
    
* [消息队列【RabbitMQ】Dotnet6示例]()

* [消息队列【RabbitMQ】死信队列]()

<!-- TODO
* [消息队列【RabbitMQ】dotnetCore MVC集成RabbitMQ]() -->


<!-- https://www.cnblogs.com/smallroll/p/17181614.html -->

多个消费者消费同一个队列的消息


* 轮询

	RabbitMQ默认将消息逐一顺序分配给各消费者，该消息分配机制称为轮询（Round-Robin）

* 消息预取

<!-- TODO
https://www.cnblogs.com/youngdeng/p/12867844.html -->


消费者一次接收一条消息，代码```channel.BasicQos(0, 1, false);```


公平分发需要消费者开启手动应答，关闭自动应答
关闭自动应答代码channel.BasicConsume("queue_test", false, consumer);
消费者开启手动应答代码：channel.BasicAck(ea.DeliveryTag, false);


### 数据丢失问题

* 启用数据持久化

	将Exchange、Queue、Message都设置了持久化

* 手动确认消息

	autoAck=False



* 镜像队列

* 消费端和服务端消息确认

