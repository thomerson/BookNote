## 常用命令

docker常用命令

***

### 容器生命周期

* ```docker run -d -P image```

  创建并启动镜像 ```create```+```start```

  * d 后台运行
  * P 开放端口

* ```start```/```stop```/```restart```

* ```kill```

* ```rm```

  删除

* ```pause```/```unpause```

  暂停/恢复暂停


* ```exec```

  在运行的容器中执行命令

***

### 容器

* ```ps -a```
  列出容器
  * a 所有容器，包括未运行的


* ```inspect```

  原信息，详细信息

* ```top```

  进程信息

* ```attach```

  连接到正在运行的容器

* ```events```

* ```logs```

* ```wait```

  阻塞运行直到容器停止

* ```export```

* ```port```

* ```stats```

  容器资源的使用情况，包括：CPU、内存、网络 I/O 等

***


### 容器rootfs命令

```rootfs```/根文件系统

* ```commit```

  从容器创建一个新的镜像⭐

* ```cp```

  用于容器与主机之间的数据拷贝⭐

* ```diff```

  检查容器里文件结构的更改⭐

***

### 本地镜像管理

* ```images``` 

  列出本地镜像

* ```rmi``` 

  删除⭐

* ```tag```

  标记本地镜像，将其归入某一仓库;版本⭐

* ```build```

  用于使用 Dockerfile **创建**镜像⭐

* ```history```

* ```save```

* ```load```

  导入使用 docker save 命令导出的镜像⭐

* ```import```

  从归档文件中**创建**镜像⭐

***

### 其他

* version

* info

  显示 Docker 系统信息，包括**镜像和容器数**⭐

***

## 镜像仓库

* pull

* push

* login

* search

