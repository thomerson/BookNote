## Dotnet6使用rabbitMQ

* Producer

    发送消息

* Consumer

    订阅消息

### Producer


1. 安装nuget包

```powershell
install-package rabbitmq.client
```

2. 创建连接，建立会话，申明队列，发送消息

```c#

using RabbitMQ.Client;
using System.Text;

namespace Demo.Rabbit.Producer
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Hello, World!");
        }

        static void SendMessage()
        {
            var factory = new ConnectionFactory()       // 创建连接工厂对象
            {
                HostName = "localhost",
                Port = 5672,
                UserName = "guest",
                Password = "guest"
            };
            var connection = factory.CreateConnection();    // 创建连接对象
            var channel = connection.CreateModel();         // 创建连接会话对象

            string queueName = "queue1";

            // 声明一个队列
            channel.QueueDeclare(
                queue: queueName,   // 队列名称
                durable: false,     // 是否持久化，true持久化，队列会保存磁盘，服务器重启时可以保证不丢失相关信息
                exclusive: false,   // 是否排他，如果一个队列声明为排他队列，该队列仅对时候次声明它的连接可见，并在连接断开时自动删除
                autoDelete: false,  // 是否自动删除，自动删除的前提是：至少有一个消费者连接到这个队列，之后所有与这个队列连接的消费者都断开时，才会自动删除
                arguments: null     // 设置队列的其他参数
            );

            var str = String.Empty;
            do
            {
                Console.WriteLine("发送内容：");
                str = Console.ReadLine()!;

                // 消息内容
                byte[] body = Encoding.UTF8.GetBytes(str);

                // 发送消息
                channel.BasicPublish("", queueName, null, body);

                // Console.WriteLine("成功发送消息：" + str);
            } while (str.Trim().ToLower() != "exit");

            channel.Close();
            connection.Close();

        }
    }
}

```


## Consumer


1. 安装nuget包

```powershell
install-package rabbitmq.client
```

2. 创建连接，建立会话，申明队列，创建消费者，监听消息

```c#
using RabbitMQ.Client;
using RabbitMQ.Client.Events;
using System.Text;

namespace Demo.Rabbit.Consumer
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.WriteLine("Consumer started!");
            SubscribeMQ();
            Console.ReadLine();
        }

        static void SubscribeMQ()
        {
            var factory = new ConnectionFactory()       // 创建连接工厂对象
            {
                HostName = "localhost",
                Port = 5672,
                UserName = "guest",
                Password = "guest"
            };
            var connection = factory.CreateConnection();    // 创建连接对象
            var channel = connection.CreateModel();         // 创建连接会话对象

            string queueName = "queue1";

            //声明一个队列
            channel.QueueDeclare(
              queue: queueName,//消息队列名称
              durable: false,//是否持久化,true持久化,队列会保存磁盘,服务器重启时可以保证不丢失相关信息。
              exclusive: false,//是否排他,true排他的,如果一个队列声明为排他队列,该队列仅对首次声明它的连接可见,并在连接断开时自动删除.
              autoDelete: false,//是否自动删除。true是自动删除。自动删除的前提是：致少有一个消费者连接到这个队列，之后所有与这个队列连接的消费者都断开时,才会自动删除.
              arguments: null ////设置队列的一些其它参数
            );

            // 创建消费者对象
            var consumer = new EventingBasicConsumer(channel);
            consumer.Received += (model, ea) => {

                byte[] message = ea.Body.ToArray();
                Console.WriteLine("接收到的消息为：" + Encoding.UTF8.GetString(message));

                // channel.BasicAck(ea.DeliveryTag, true); // 开启返回消息确认 手动模式
            };

            // 消费者开启监听
            channel.BasicConsume(queueName, true, consumer);

            Console.ReadKey();
            channel.Dispose();
            connection.Close();

        }
    }
}
```