### Redis 简介

Redis：[Remote Dictionary Server](https://redis.io/)(远程字典服务器)，是完全开源的，使用ANSIC语言编写遵守BSD协议，是一个高性能的Key-Value数据库提供了丰富的数据结构，例如String、Hash、List、Set、SortedSet等等。数据是存在内存中的，同时Redis支持事务、持久化、LUA脚本、发布/订阅、缓存淘汰、流技术等多种功能特性提供了主从模式、Redis Sentinel和Redis Cluster集群架构方案.

> Redis之父Salvatore Sanfilippo,Github: https://github.com/antirez Blog: http://antirez.com/latest/0
>
> 中文文档：https://www.redisio.com/ https://redis.com.cn/ 中文命令文档：http://doc.redisfans.com/

### Redis 功能

1. 分布式缓存

2. 内存存储和持久化（RDB+AOF）,Redis 支持异步将内存中的数据写到硬盘上，不影响 Redis 读写服务

3. 高可用架构搭配 单机、主从、哨兵、集群

   

![无标题](http://image.wangxiaohuan.com/blog/image/202408051738930.png)



### Redis 优点

1. 性能极高 – Redis能读的速度是110000次/秒,写的速度是81000次/秒
2. Redis 数据类型丰富，不仅仅支持简单的 key-value 类型的数据，同时还提供 list，set，zset，hash等数据结构的存储
3. Redis支持数据的持久化，可以将内存中的数据保持在磁盘中，重启的时候可以再次加载进行使用
4. Redis支持数据的备份，即master-slave模式的数据备份

![无标题](http://image.wangxiaohuan.com/blog/image/202408051746461.png)

>Redis从发布至今，已经有十余年的时光了，一直遵循着自己的命名规则：
>
>版本号第二位如果是奇数，则为非稳定版本 如2.7、2.9、3.1
>
>版本号第二位如果是偶数，则为稳定版本 如2.6、2.8、3.0、3.2
>
>当前奇数版本就是下一个稳定版本的开发版本，如2.9版本是3.0版本的开发版本
>
>我们可以通过redis.io官网来下载自己感兴趣的版本进行源码阅读：
>
>历史发布版本的源码：https://download.redis.io/releases/

 

