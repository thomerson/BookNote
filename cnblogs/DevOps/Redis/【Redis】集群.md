## 集群

### 集群特点

* 高并发
  
  分担服务器压力，提供并发量

* 高可用
  
  * 解决单点问题

  * 系统伸缩性（增删节点）


### 集群类型

* 对称集群

  集群实例角色地位相同，功能完全一样，**无状态**，特点：**数据计算**

* 非对称集群

  集群实例角色地位不相同，功能不一样（主从/读写），**有状态**，特点：**数据存储**


Redis集群属于非对称集群

### Redis集群发展

* Redis第一代集群：**主从master-slaver**

    * 问题： Master宕机

* Redis第二代集群：**哨兵模式**
    
    * 优点： master宕机后选举新的master
    
    * 问题：Master压力大

* Redis第三代集群：**Redis-cluster** 去中心化

    * ```redis-trib.rb``` redis集群工具包

    * 依赖：**ruby**



## Redis-cluster搭建

1. redis安装

2. redis集群配置```redis.windows.conf```

    * cluster-enabled yes #启用集群

    * port #端口

    * appendOnly yes #数据保存为aof格式


3. redis集群搭建工具```redis-trib.rb```

    1. 安装[```Ruby```](https://rubyinstaller.org/downloads/)运行环境

       添加环境变量```ruby --version```验证安装成功
        

    2. ```Ruby```下[Redis驱动](https://rubygems.org/gems/redis)下载

       在redis-cluster工作路径下cmd安装```gem install --local ...redis.gem```

    3. redis集群工具[```redis-trib.rb```](https://github.com/beebol/redis-trib.rb)下载

       输入命令```redis-trib.rb create --replicas 1 127.0.0.1:6380 127.0.0.1:6381 127.0.0.1:6382 127.0.0.1:6383```

       replicas 参数表示每个主节点后面有1个子节点


### redis-trib命令

* create 

* check

* info

* fix 修复

* reshared 在线迁移slot

* rebalance 平衡集群节点slot数量

* add-node

* del-node

* set-timeout


## 整个集群不可用

* 主节点和从节点同时宕机

* 有两个主节点宕机了







