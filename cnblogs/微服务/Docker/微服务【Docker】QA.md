## docker使用中的一些问题和思考

1. run和start命令的区别

    * run命令只在第一次运行镜像操作时使用，相当于执行了两步操作，**将镜像放入容器中然后将容器启动**
    
    * 而start命令在重新启动已经存在的镜像时使用，使用该命令需要知道容器的id或者名字。

    