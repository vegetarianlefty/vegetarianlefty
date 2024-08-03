### RDB 介绍

RDB 是在指定的时间间隔执行数据集的时间点快照。就是把某一时刻的数据和状态以文件的形式写到磁盘上，也就是快照。这样一来即使故障宕机，快照文件也不会丢失，数据的可靠性也就得到了保证。这个快照文件就称为RDB文件(dump.rdb)，其中，RDB就是Redis DataBase的缩写。

### 作用

Redis 的数据都在内存中，在保存备份时，执行全量快照，把内存中的数据都写入到磁盘中，生成 RDB 文件。

### Redis.conf 配置文件

RDB 配置文件说明，在 Redis.conf 配置文件中 SNAPSHOTTING 

##### save <seconds> <changes>：表示 m 秒内数据集存在 n 次修改，就将触发 bgsave。生成新的 RDB 文件

##### dbfilename : RDB 文件名字

##### dir： RDB 文件路径

##### stop-writes-on-bgsave-error ：默认yes

如果配置成no，表示你不在乎数据不一致或者有其他的手段发现和控制这种不一致，那么在快照写入失败时，

也能确保redis继续接受新的写请求

##### rdbcompression：默认yes

对于存储到磁盘中的快照，可以设置是否进行压缩存储。如果是的话，redis会采用LZF算法进行压缩。 如果你不想消耗CPU来进行压缩的话，可以设置为关闭此功能

##### rdbchecksum：默认yes

在存储快照后，还可以让redis使用CRC64算法来进行数据校验，但是这样做会增加大约10%的性能消耗，如果希望获取到最大的性能提升，可以关闭此功能

##### rdb-del-sync-files：默认no

在没有持久性的情况下删除复制中使用的RDB文件启用。默认情况下no，此选项是禁用的。

```
Redis 6.0.16 版本以下配置 
## 下配置 save 参数，来触发 Redis 的 RDB 持久化条件配置介绍 
save m n:表示 m 秒内数据集存在 n 次修改，就将触发 bgsave。生成新的 RDB 文件
save 900 1 ：表示每隔 900 秒内数据集存在 1 次修改，就触发命令，生成新的 RDB 文件。
 
 ################################ SNAPSHOTTING  ################################
#
# Save the DB on disk:
#
#   save <seconds> <changes>
#
#   Will save the DB if both the given number of seconds and the given
#   number of write operations against the DB occurred.
#
#   In the example below the behaviour will be to save:
#   after 900 sec (15 min) if at least 1 key changed
#   after 300 sec (5 min) if at least 10 keys changed
#   after 60 sec if at least 10000 keys changed
#
#   Note: you can disable saving completely by commenting out all "save" lines.
#
#   It is also possible to remove all the previously configured save
#   points by adding a save directive with a single empty string argument
#   like in the following example:
#
#   save ""

save 900 1
save 300 10
save 60 10000


Redis 6.0.16 版本以上配置
################################ SNAPSHOTTING  ################################

# Save the DB to disk.
#
# save <seconds> <changes> [<seconds> <changes> ...]
#
# Redis will save the DB if the given number of seconds elapsed and it
# surpassed the given number of write operations against the DB.
#
# Snapshotting can be completely disabled with a single empty string argument
# as in following example:
#
# save ""
#
# Unless specified otherwise, by default Redis will save the DB:
#   * After 3600 seconds (an hour) if at least 1 change was performed
#   * After 300 seconds (5 minutes) if at least 100 changes were performed
#   * After 60 seconds if at least 10000 changes were performed
#
# You can set these explicitly by uncommenting the following line.
#
# save 3600 1 300 100 60 10000

# 自定义保存路径修改 

# The working directory.
#
# The DB will be written inside this directory, with the filename specified
# above using the 'dbfilename' configuration directive.
#
# The Append Only File will also be created inside this directory.
#
# Note that you must specify a directory here, not a file name.
## 默认配置
dir ./ 
#例如
dir /data/redis/dumpfiles

##自定义文件名称
# The filename where to dump the DB
##默认配置
dbfilename dump.rdb
##例如
dbfilename dev-dump-redis.rdb
```

### RDB 触发操作

#### 自动触发

只要达到配置文件中默认的快照配置，就会自动触发

#### 手动触发

save : 在主程序中执行会阻塞当前 Redis 服务器，直到持久化工作完成，在执行 save 命令期间，Redis 不能处理其它命令，生产环境禁止使用。

bgsave ： Redis 会在后台一步进行快照操作，不阻塞当前 Redis 服务器工作，在进行 RDB 快照期间，还可以响应客户端请求，该触发方式会 fork 一个子进程，由子进程复制持久化过程。

> [!TIP]
>
> 执行 flushall/flushdb 命令也会产生 dump.rdb 文件，无意义，空文件
>
> 执行 shutdown 命令，并且没有设置开启 AOF 持久化，也会产生 dump.rdb 文件
>
> 主从复制是，主节点自动触发，也会产生 dump.rdb 文件

> [!IMPORTANT]
>
> 禁用 RDB 快照 
>
> 使用命令 redis-cli config set save "" 或者修改 Redis.conf 配置文件中，save ""



> [!WARNING]
>
> 不可以把备份文件dump.rdb和生产redis服务器放在同一台机器，必须分开各自存储，
>
> 以防生产机物理损坏后备份文件也挂了。



#### RDB 恢复

将备份 RDB 文件（dump.rdb）移动到 Redis 安装目录，并启动服务即可恢复备份的数据。

### 优点

- 适合大规模的数据恢复

- 按照业务定时备份
- 对数据完整性和一致性要求不高
- RDB 文件在内存中的加载速度要比 AOF 快得多

### 缺点

- 在一定问隔时间做一次备份，所以如果redis意外down掉的话，就会丢失从当前至最近一次快照期间的数据，快照之间的数据会丢失
- 内存数据的全量同步，如果数据量太大会导致I/0严重影响服务器性能
- RDB依赖于主进程的fork，在更大的数据集中，这可能会导致服务请求的瞬间延迟。
- fork的时候内存中的数据被克隆了一份，大致2倍的膨胜性，需要考虑

### RDB 检查修复 dump.rdb 文件

```
# 进入bin 目录
cd /usr/local/bin
#执行命令
redis-check-rdb ./redis/dumpfiles/dump.rdb
```

