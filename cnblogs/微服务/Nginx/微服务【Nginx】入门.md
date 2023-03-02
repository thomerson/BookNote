## Nginx

**反向代理**，高并发，**负载均衡**

基于事件驱动机制


### 常用功能

* [反向代理](#反向代理)

* [负载均衡](#负载均衡)

* web缓存

### 反向代理

```Reverse Proxy```

![反向代理](https://img-blog.csdnimg.cn/619d428416b44f538d3da146bd5c52fc.png?x-oss-process=image/watermark,type_d3F5LXplbmhlaQ,shadow_50,text_Q1NETiBA5Lya6aOe55qE54yr5LiN5ZCD6bG8,size_20,color_FFFFFF,t_70,g_se,x_16)

* 正向代理

    **浏览器中**/请求端配置代理服务器

* 反向代理

    在服务器中配置代理服务器

### 负载均衡

分摊到多个操作单元上进行执行


策略

* 轮询（默认）

    适合服务器配置相当，无状态且短平快的服务使用。也适用于图片服务器集群和纯静态页面服务器集群

* 指定轮询几率/权重

    指定轮询几率，weight和访问比率成正比，用于后端服务器性能不均的情况

* ```ip_hash```

    如果客户已经访问了某个服务器，当用户再次访问时，会将该请求通过哈希算法，自动定位到该服务器。每个请求按访问ip的hash结果分配，这样每个访客固定访问一个后端服务器，可以解决session的问题


### Nginx配置

* ```worker_connections```  

    #最大连接数，默认为1024(早期是512)

* http块

    可以嵌套多个server，配置代理，缓存，日志定义等绝大多数功能和第三方模块的配置

    * server块

        * listen
            
        #配置监听端口 默认80
        
        * server_name    
        
        #配置服务器名


### Master-Worker模式

启动Nginx后，其实就是在80端口启动了Socket服务进行监听

![master-worker模式](https://pic4.zhimg.com/80/v2-b24eb2b29b48f59883232a58392ddae3_720w.webp)


* Master进程

    * 读取并验证配置文件nginx.conf
    
    * 管理worker进程

* Worker进程

    * 每一个Worker进程都维护一个线程（避免线程切换），处理连接和请求



### Nginx热部署

热部署就是修改配置文件后，不需要重启，不需要中断请求，就能让配置文件生效

修改配置文件nginx.conf后，重新生成新的worker进程，以新的配置进行处理请求，而且新的请求必须都交给新的worker进程，至于老的worker进程，等把那些以前的请求处理完毕后kill掉



<!-- https://www.runoob.com/w3cnote/nginx-setup-intro.html -->


