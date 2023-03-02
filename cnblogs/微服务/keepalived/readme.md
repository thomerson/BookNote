## keepalived

使用C语言编写的路由热备软件

```LVS```/```Linux Virtual Server```/Linux虚拟服务器 负载均衡

实现高可用的```VRRP```功能，作为其他服务（例如：Nginx，Haproxy，MySQL等）的高可用解决方案软件


### VRRP

```Virtual Router Redundancy Protocol```/虚拟路由器冗余协议

解决静态路由单点故障问题的，它能够保证当个别节点宕机时，整个网络可以不间断地运行。

通过一种竞选协议机制将路由任务交给某台VRRP路由器

工作时主节点发包，备节点接包，当备节点接收不到主节点发的数据包时，就启动接管程序接管主节点的资源。备节点可以有多个，通过优先级竞选，但一般Keepalived系统工作中都是一对。

### keepalived实现高可用

Keepalived高可用之间是通过VRRP进行通信的，VRRP是通过竞选机制来确定主备的，主的优先级高于备，因此，工作时主会优先获得所有的资源，备节点处于等待状态，当主挂了的时候，备节点就会接管主节点的资源，然后顶替主节点对外提供服务。

在Keepalived服务之间，只有作为主的服务器会一直发送VRRP广播包，告诉备它还活着，此时备不会抢占主，当主不可用时，即备监听不到主发送的广播包时，就会启动相关服务接管资源，保证业务的连续性。接管速度最快可以小于**1**秒。

## 安装及配置

<!-- TODO

https://mp.weixin.qq.com/s?__biz=MzI3NzQ4MTE4Mw==&mid=2247484964&idx=2&sn=1faedd52a4c6e5694422705942f665af&chksm=eb64d594dc135c82de5c220a1741b42a3bd1ac706c31796b5dbf7cbee7ef61152e523f2644aa&cur_album_id=1337035688499380226&scene=189#wechat_redirect -->

## 高可用实例

<!-- TODO

https://mp.weixin.qq.com/s?__biz=MzI3NzQ4MTE4Mw==&mid=2247484988&idx=1&sn=795bb2035087163ee3da3333b346eca8&chksm=eb64d58cdc135c9aaf9b8f52587abba6783c0e6506231c6263c218a8c368403bc0a76db79d67&cur_album_id=1337035688499380226&scene=189#wechat_redirect -->

## 集成Nginx实现高可用集群

<!-- TODO

https://mp.weixin.qq.com/s?__biz=MzI3NzQ4MTE4Mw==&mid=2247485081&idx=1&sn=36f25bc47332565ce778b77bec5ec68b&chksm=eb64d529dc135c3f9558d3b2e400387db0367f5515957d8d373decc9e6ee699306f4b5ca8353&cur_album_id=1337035688499380226&scene=189#wechat_redirect -->