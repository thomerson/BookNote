## rabbitMQ

1. 安装Erlang环境

安装完成后配置erl环境变量

输入cmd命令，验证安装成功

```erl -version```



2. 安装rabbitMQ server

安装默认路径


```C:\Program Files\RabbitMQ Server\rabbitmq_server-3.10.7```

在这个路径下执行cmd命令(可以在这个路径地址栏输入cmd)
```rabbitmq-plugins enable rabbitmq_management```
安装管理页面的插件


3. 启动rabbitMQ server


在这个路径下执行cmd命令启动```rabbitmq-server.bat```

或者查看service中RabbitMQ的服务启动

4. 地址栏输入```http://localhost:15672```，打开管理页面

注意步骤2中安装插件，不然打不开这个页面
默认账号密码```guest/guest```







