## ELK

日志中心：收集日志

* ElasticSearch

存储日志

* Logstash

收集日志

默认同步收集，性能较差，所以使用rabbitMQ改成异步收集日志

* Kibana

展示日志


各个系统->rabbitMQ->Logstash->ElasticSearch->Kibana



## 记录日志步骤

1. 发送日志到rabbitMQ

2. rabbitMQ发送日志到Logstash

3. Logstash发送日志到ES

4. Kibana展示ES

[Demo](https://github.com/thomerson/Demo/tree/main/dotnet6/gatlin.demo.shopping)


## 微服务记录日志到ES系列

0. [微服务【ELK】入门](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/微服务【ELK】入门.md)

1. [消息队列【rabbitMQ】安装](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/消息队列【rabbitMQ】安装.md)


2. [微服务【ELK】elasticsearch安装启动](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/微服务【ELK】elasticsearch安装启动.md)

3. [微服务【ELK】LogStash安装启动](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/微服务【ELK】LogStash安装启动.md)

4. [微服务【ELK】LogStash安装启动](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/微服务【ELK】LogStash安装启动.md)

5. [微服务【ELK】Kibana安装启动](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/微服务【ELK】Kibana安装启动.md)

6. [消息队列【rabbitMQ】dotnet6记录日志](https://github.com/thomerson/BookNote/blob/master/cnblogs/微服务/消息队列【rabbitMQ】dotnet6记录日志.md)

