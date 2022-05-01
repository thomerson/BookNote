## 关键概念
* 开放源码的类 UNIX 操作系统
* Linux: Linux is not Unix

## CentOS

## 远程登录

### putty


## 文件基本属性

### chown

change owner 修改所属用户和组

### chmod 

change mode 修改用户权限

### chgrp


## 用户组
* owner 拥有者
* group 组
* others 其他

### 权限
* read
* write
* execute

## 文件目录管理

### 绝对路径

由根目录```/``` 写起

### 相对路径

例如 ```../man```

### 命令

* ls（英文全拼：list files）: 列出目录及文件名
* cd（英文全拼：change directory）：切换目录
* pwd（英文全拼：print work directory）：显示目前的目录
* mkdir（英文全拼：make directory）：创建一个新的目录
* rmdir（英文全拼：remove directory）：删除一个空的目录
* cp（英文全拼：copy file）: 复制文件或目录
* rm（英文全拼：remove）: 删除文件或目录
* mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称

### 文件内容查看

* cat  由第一行开始显示文件内容
* tac  从最后一行开始显示，可以看出 tac 是 cat 的倒着写！
* nl   显示的时候，顺道输出行号！
* more 一页一页的显示文件内容
* less 与 more 类似，但是比 more 更好的是，他可以往前翻页！
* head 只看头几行
* tail 只看尾巴几行

## 用户和用户组管理

### 用户管理

* useradd 
* userdel
* usermod
* passwd


### 用户组管理

* groupadd
* groupdel
* groupmod

### 与用户账号有关的系统文件

* /etc/passwd
所有用户都是可读

* /etc/shadow
真正的加密后的用户口令字存放

* /etc/passwd
只存放一个特殊的字符，例如“x”或者“*”

* /etc/group

## 磁盘管理

* df（英文全称：disk full）：列出文件系统的整体磁盘使用量
* du（英文全称：disk used）：检查磁盘空间使用量
* fdisk：用于磁盘分区

### 磁盘格式化

* mkfs（make filesystem）

### 磁盘检验

