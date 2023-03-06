## select

轮询

耗费资源，时间慢

## epoll

是为解决Linux内核处理大量文件描述符而提出的方案

属于Linux下多路I/O复用接口中select/poll的增强

经常应用于Linux下高并发服务型程序，特别是在大量并发连接中只有少部分连接处于活跃下的情况 (通常是这种情况)，在该情况下能显著的提高程序的CPU利用率

<!-- TODO -->
<!-- https://zhuanlan.zhihu.com/p/427512269 -->