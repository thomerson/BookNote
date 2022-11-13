## jenkins 安装

* windows安装

  需要java1.7，不支持java8

* docker安装

    1. 启动docker服务

       ```service docker start```

    2. 下载jinkens镜像

       ```docker pull jenkins/jenkins```

    3. 创建Jenkins挂载目录并授权权限

        ```mkdir -p /var/jenkins_mount```

        ```chmod 777 /var/jenkins_mount```

    4. 创建并启动Jenkins容器

       ```docker run -d -p 10240:8080 -p 10241:50000 -v /var/jenkins_mount:/var/jenkins_home -v /etc/localtime:/etc/localtime --name myjenkins jenkins/jenkins```


       * -d 后台运行镜像
       
       * -p 10240:8080 将镜像的8080端口映射到服务器的10240端口。
       
       * -p 10241:50000 将镜像的50000端口映射到服务器的10241端口
       
       * -v /var/jenkins_mount:/var/jenkins_mount /var/jenkins_home目录为容器jenkins工作目录，我们将硬盘上的一个目录挂载到这个位置，方便后续更新镜像后继续使用原来的工作目录。这里我们设置的就是上面我们创建的 /var/jenkins_mount目录
       
       * -v /etc/localtime:/etc/localtime让容器使用和服务器同样的时间设置。
       
       * --name myjenkins 给容器起一个别名


    5. 查看jenkins是否启动成功

       ```docker ps -l```查看容器列表


    6. 查看jenkins日志

       ```docker logs myjenkins```

    7. 配置镜像加速

      进入到jenkins_mount目录```cd /var/jenkins_mount/```

      修改```vi  hudson.model.UpdateCenter.xml```里的内容

      将 url 修改为 清华大学官方镜像：```https://mirrors.tuna.tsinghua.edu.cn/jenkins/updates/update-center.json```

    8. 访问Jenkins页面，输入ip加上10240

       第一次访问页面提示需要管理员密码，在指定路径上可以找到

## 安装jenkins插件






