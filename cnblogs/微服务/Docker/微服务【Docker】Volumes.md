## Volumes


默认容器的数据的读写发生在容器的存储层，当容器被删除时其上的数据将会丢失。

**数据卷**实现了数据的持久化存储

是一个可供一个或多个容器使用的位于宿主机上特殊目录


* 数据卷可以在容器间共享和重用 

* 对数据卷的写入操作，不会对镜像有任何影响 

* 数据卷默认会一直存在，即使容器被删除

* 使用数据卷的目的是持久化容器中的数据，以在容器间共享或者防止数据丢失（写入容器存储层的数据会丢失）

### 使用数据卷的步骤

1. 创建数据卷

    ```shell
    # 创建
    $ docker volume create my-vol
    # 查看volume
    $ docker volume ls
    # 查看volume详细信息
    $ docker volume inspect my-vol
    # 删除
    $ docker volume rm my-vol
    # 删除所有未使用的 Volumes
    $ docker volume prune
    ```

2. 挂载或者绑定容器指定目录

    使用 ```-v``` 或 ```--mount``` 参数将数据卷挂载容器指定目录中，这样所有该容器针对该指定目录的写操作都会保存在宿主机上的 Volume 中

    * 使用```--mount```参数

    ```shell
    # source 指定 volume，destination 指定容器内的文件或文件夹
	$ docker run -d \
	
	--name=nginxtest \
	
	--mount source=nginx-vol,destination=/usr/share/nginx/html \

    # --mount source=nginx-vol,destination=/usr/share/nginx/html,readonly \ # 只读
	
	nginx:latest

    ```

    * 使用 ```-v``` 参数

    ```shell
	$ docker run -d \
	
	--name=nginxtest \
	
	-v nginx-vol:/usr/share/nginx/html \
    # -v nginx-vol:/usr/share/nginx/html:ro \ # 只读
	
	nginx:latest
    ```

### volume mount和bind mount区别

volume和bind mount都是持久化容器的机制

**volume是由docker来进行管理的，而bind mount完全是依赖于主机的目录结构和操作系统**

volume 相对于 bind mount的优点
 
* volume更加容易进行备份和迁移
* 可以通过docker客户端命令或者docker api来管理volume (比如：docker volume命令)
* volume可以在linux和windows容器中运行
* volume可以更加安全的在多个容器之间进行共享
* volume驱动程序允许在远程主机或云提供商上存储卷，以加密volume的内容或添加其他功能
* 新volume的内容可以由容器预先填充
* Docker Desktop上的卷比Mac和Windows主机上的绑定挂载具有更高的性能。

![volumes-mount](https://cdn.nlark.com/yuque/0/2022/png/12894334/1661306490806-50c23619-7a99-4eb4-947e-50c249ef18bf.png)

