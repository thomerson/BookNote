## 关键概念
* 开放源码的类 UNIX 操作系统
* Linux英文解释： Linux is not Unix

## redhat debian

* redhat 
    * CentOS
主要用于服务器版本，没有UI

* debian
    * Ubuntu

视窗界面良好，民用


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

* fsck（file system check）

### 磁盘挂载与卸除

* mount 挂载
* umount 卸除

## vi/vim

文本编辑器

### 三种模式

* 命令模式（Command mode）
* 输入模式（Insert mode）
* 底线命令模式（Last line mode）


## yum 命令

> Yellow dog Updater, Modified  
> Shell 前端软件包管理器

1. 列出所有可更新的软件清单命令：yum check-update

2. 更新所有软件命令：yum update

3. 仅安装指定的软件命令：yum install <package_name>

4. 仅更新指定的软件命令：yum update <package_name>

5. 列出所有可安裝的软件清单命令：yum list

6. 删除软件包命令：yum remove <package_name>

7. 查找软件包命令：yum search <keyword>

8. 清除缓存命令:

    * yum clean packages: 清除缓存目录下的软件包
    * yum clean headers: 清除缓存目录下的 headers
    * yum clean oldheaders: 清除缓存目录下旧的 headers
    * yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的 headers

## apt 命令

> Advanced Packaging Tool
> Shell 前端软件包管理器

1. 列出所有可更新的软件清单命令：sudo apt update

2. 升级软件包：sudo apt upgrade

3. 列出可更新的软件包及版本信息：apt list --upgradeable

* 升级软件包，升级前先删除需要更新软件包：sudo apt full-upgrade

* 安装指定的软件命令：sudo apt install <package_name>

* 安装多个软件包：sudo apt install <package_1> <package_2> <package_3>

* 更新指定的软件命令：sudo apt update <package_name>

* 显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：sudo apt show <package_name>

* 删除软件包命令：sudo apt remove <package_name>

* 清理不再使用的依赖和库文件: sudo apt autoremove

* 移除软件包及配置文件: sudo apt purge <package_name>

* 查找软件包命令： sudo apt search <keyword>

* 列出所有已安装的包：apt list --installed

* 列出所有已安装的包的版本信息：apt list --all-versions



## [Linux命令大全](https://www.runoob.com/linux/linux-command-manual.html)

### 命令说明

* sudo 

系统管理指令，是允许系统管理员让普通用户执行一些或者全部的root命令的一个工具



* ls 

list files，显示指定工作目录下之内容

* cd

Change Directory,切换目录

```cd ..``` 返回到上一层目录

* rmdir

remove Directory,删除空的目录

* rm

remove,移除文件或目录

* mkdir

make directory，创建新的目录

* pwd

Print Working Directory，显示目前所在目录的命令

* cp

copy,复制文件或目录

* mv 

move,移动文件与目录，或修改名称







