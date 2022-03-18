## 说明
平时遇到的基础问题的QA和总结，不定期更新

## 目录
TODO

## 计算机单位

### bit byte
* bit:**位**,电脑记忆体中最小的单位，在二进位电脑系统中，每一bit 可以代表0 或 1 的数位讯号,**小写 b 代表 bit**
* byte:**字节**， 一个Byte由8 bits 所组成，可代表一个字元(A\~Z)、数字(0\~9)、或符号(,.?!%&+-*/)，是记忆体储存资料的基本单位,**大写 B代表 Byte**
每个中文字则须要**两Bytes** ⭐


### kb mb gb tb 
1KB = 1024byte 千/Kilobyte
1MB = 1024KB 兆/一百万/million/MByte
1GB = 1024MB 千兆/十亿/Gigabyte
1TB = 1024GB 兆兆/terabyte

### bps Bps
* bps :bits per second 一般数据机及网络通讯的传输速率都是以「bps」为单位。如56Kbps、100.0Mbps 等等
* Bps :Byte per second 电脑一般都以Bps 显示速度，如1Mbps 大约等同 128 KBps

## 信息系统

### ERP CRM SAP
* ERP: Enterprise Resource Planning/企业资源计划      
* CRM: Customer Relationship Management/客户关系管理      
* SCM: Supply Chain Management/供应链管理     
* SAP: System Applications and Products/是SAP【思爱普】公司的产品——企业管理解决方案的软件名称 是全世界排名第一的ERP软件


### DFD ERD DD SSD
* DFD：data flow diagram/数据流图
* ERD: Entity relation diagram/实体关系图
* DD: data dictionary/数据字典
* SSD: system struct diagram/系统结构图


### MAC IP domain
* MAC: media access control/媒体访问控制地址/**物理地址**,长度为**48**位，**烧录在网卡上**，**全世界唯一**
* IP: Internet protocal address/互联网协议地址/是一种**逻辑地址**
    * IPV4,长度**32**位
    * IPv6,长度位**64**位
* domain： 域名，IP地址不方便记忆，通过**域名系统**/**DNS**/**Domain Name System**/将域名和IP地址相互映射

### SaaS Paas IaaS
* SaaS/soft as a service
    > 软件即服务：软件的开发管理部署都交给服务商，不需要关心技术问题，拿来即用，例如email,erp系统
* PaaS/Platform as a service
    > 平台即服务：提供软件部署平台（runtime），抽象掉了硬件和操作系统细节，可以无缝地扩展.开发者只需要关注自己的业务逻辑，不需要关注底层
    * APaaS/application platform as a service：Tomcat，NodeJs，windows 等
    * IPaaS/integration as a service：数据库（MySQL,redis,mongoDB）,消息队列，Memcache等
* IaaS/Infrastructure as a service
    > 基础设施服务：云服务的最底层，主要提供一些基础资源，可以根据需要定制cpu，内存，系统等。例如阿里云

> 参考:[IaaS，PaaS，SaaS 的区别](https://ruanyifeng.com/blog/2017/07/iaas-paas-saas.html)


