### Redis 集群搭建步骤

准备 3 台服务器，安装 Redis 数据库，

3 主 3 从 Redis 集群搭建

创建 Redis 实例启动配置文件 ,并启动

```
# 创建实例配置文件目录
mkdir -p /data/services/redis/cluster
# 创建 log 日志文件目录
mkdir -p /data/log/redis/cluster
#创建启动命令 bin.sh
vim /data/services/redis/cluster/bin6380.sh
#创建启动命令 bin.sh 内容
/usr/local/bin/redis-server /data/services/redis/cluster/redisCluster6380.conf
。。。。。以此类推 

#192.168.1.201
vim /data/services/redis/cluster/bin6380.sh
vim /data/services/redis/cluster/bin6381.sh
vim /data/services/redis/cluster/redisCluster6380.conf
vim /data/services/redis/cluster/redisCluster6381.conf
#192.168.1.202
vim /data/services/redis/cluster/bin6382.sh
vim /data/services/redis/cluster/bin6383.sh
vim /data/services/redis/cluster/redisCluster6382.conf
vim /data/services/redis/cluster/redisCluster6383.conf
#192.168.1.203
vim /data/services/redis/cluster/bin6384.sh
vim /data/services/redis/cluster/bin6385.sh
vim /data/services/redis/cluster/redisCluster6384.conf
vim /data/services/redis/cluster/redisCluster6385.conf

#在不同服务器，启动Redis 实例,
sh bin6380.sh 
。。。。。以此类推
```

redisCluster6380 配置文件内容,以此类推注意 端口 和 日志 pid red aof nodes 名称 

```
bind 0.0.0.0
daemonize yes
protected-mode no
port 6380
logfile "/data/log/redis/cluster/cluster6380.log"
pidfile /data/services/redis/cluster/cluster6380.pid
dir /data/services/redis/cluster
dbfilename dump6380.rdb
appendonly yes
appendfilename "appendonly6380.aof"
requirepass 123456
masterauth 123456
 
cluster-enabled yes
cluster-config-file nodes-6380.conf
cluster-node-timeout 5000
```

通过redis-cli 命令构建集群关系 。--cluster-replicas 1 表示为每个master创建一个slave节点 

```
### 注意自己的真实IP地址 
redis-cli -a 123456 --cluster create --cluster-replicas 1 192.168.1.201:6380 192.168.1.201:6381 192.168.1.202:6382 192.168.1.202:6383 192.168.1.203:6384 192.168.1.203:6385
```

<img src="http://image.wangxiaohuan.com/blog/image/202410141521860.png" alt="image-20241014152110357" style="zoom:50%;" />

Redis集群命令

info replication

- **功能**：在Redis主从复制配置中，该命令用于显示关于复制（replication）的信息，包括当前节点是主节点还是从节点，以及从节点的连接状态等。
- **输出**：在集群模式下，如果节点是集群的一部分而不是传统的主从复制设置的一部分，你可能会看到较少的相关信息，或者看到关于集群节点间通信的通用信息。

cluster info

**功能**：`CLUSTER INFO`命令用于获取关于Redis集群的一般信息。

**输出**：该命令的输出包含了集群的状态（例如：`ok`表示集群是健康的，`fail`表示集群遇到了问题），集群中配置的节点数量，当前可用的槽（slots）数量，以及集群中失败的节点数量等。示例输出可能如下所示：

```
cluster_state:ok  
cluster_slots_assigned:16384  
cluster_slots_ok:16384  
cluster_slots_pfail:0  
cluster_slots_fail:0  
cluster_known_nodes:6  
cluster_size:3  
cluster_current_epoch:6  
cluster_my_epoch:1  
cluster_stats_messages_ping_sent:429  
cluster_stats_messages_pong_sent:440  
...
```

cluster nodes

**功能**：`CLUSTER NODES`命令用于获取关于集群中每个节点的详细信息。

**输出**：该命令提供了每个节点的ID、IP地址和端口号、角色（主节点或从节点）、节点负责的槽范围、节点状态（例如在线或失败）、节点间的连接信息等。示例输出可能如下所示：

```

5d1ce8340f9d480e5877654c704654db87ee2554 192.168.1.116:6382@16382 master,fail - 1728887467600 1728887466296 3 connected 5461-10922
a446dcacd7fe9dc4997c4c6d53148ea1e4c04f7b 192.168.1.117:6384@16384 master,fail - 1728887472415 1728887469907 5 connected 10923-16383
ff28cef1a1ae3ee5d2a2ee86030fbd00edd3459d 192.168.1.203:6385@16385 slave,fail? 5d1ce8340f9d480e5877654c704654db87ee2554 1728887472415 1728887469906 3 connected
c5ee92c9e51e6c3baf20dc9a3ff92988c9fc6b7e 192.168.1.202:6383@16383 slave,fail? 51089918a4eacf1fd22c5bf948aba8138feccef8 1728887468603 1728887466195 1 connected
51089918a4eacf1fd22c5bf948aba8138feccef8 192.168.1.201:6380@16380 myself,master - 0 1728887485000 1 connected 0-5460
8e9bce11172060390e9e9ed46fddd34dff51b77d 192.168.1.201:6381@16381 slave a446dcacd7fe9dc4997c4c6d53148ea1e4c04f7b 0 1728887485656 5 connected
```

在这个例子中，每个节点的信息包括节点ID、地址和端口、角色、集群节点配置纪元（epoch）、最后与节点通信的时间戳、节点在集群中的配置编号（config epoch），以及它负责的槽范围。

> [!IMPORTANT]
>
> 各个机器的IP 端口要全部放开，互相能访问 或者关闭防火墙
>
> 查看所有打开的端口
> firewall-cmd --zone=public --list-ports
>
> 打开端口
>
> firewall-cmd --zone=public --add-port=端口号/tcp --permanent
>
> 配置立即生效
> firewall-cmd --reload 
>
> 重启防火墙      
> systemctl restart firewalld.service



### Redis 集群主从容错切换

### Redis 集群主从扩容

### Redis 集群主从缩容
