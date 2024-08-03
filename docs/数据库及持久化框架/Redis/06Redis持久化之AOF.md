### AOF 介绍

Redi s的 AOF（Append Only File）是一种持久化机制，旨在通过记录 Redis 服务器接收到的所有写操作命令来保证数据的可靠性和持久性。

> [!NOTE]
>
> AOF文件中的内容是以Redis命令的序列化形式存储的，文件结构简单，每一行代表一个Redis命令。
>
> 默认情况下Redis 是没有开启 AOF 的，开启需要更改配置 appendonly yes

### 作用

AOF是一种日志文件，用于记录Redis服务器接收到的所有写操作命令。它以追加的方式将写操作命令追加到文件末尾，因此也被称为追加日志。

### Redis.conf 配置文件

AOF 配置文件说明，在 Redis.conf 配置文件中 

**appendonly** :控制是否启用AOF持久化 默认no 

**appendfilename** ：设置AOF文件的名称，**默认值**：appendonly.aof**配置示例**：`appendfilename "myappendonly.aof"`

> [!TIP]
>
> Redis6 只有一个，Redis7 基于Multi Part AOF的设计，分为三个类型的文件 base 基本文件，incr增量文件，manifest清单文件
>
> ![image-20240924143901565](http://image.wangxiaohuan.com/blog/image/202409241439398.png)

**appendfsync**：控制AOF文件的同步策略

- always：每个写命令都同步到磁盘。这提供了最高的数据安全性，但可能会对性能产生较大影响。
- everysec：每秒同步一次。这是默认设置，它提供了较好的性能与数据安全性之间的平衡。
- no：依赖操作系统自身的调度进行同步。这种方式性能最好，但可能会增加数据丢失的风险。

**no-appendfsync-on-rewrite**：控制AOF重写期间是否进行fsync操作。

> [!CAUTION]
>
> 当设置为`yes`时，在AOF重写期间不进行fsync操作，这可能会降低性能损耗，但增加了数据丢失的风险。如果设置为`no`，则重写期间也会进行fsync操作，确保数据安全，但可能会增加性能开销。

**auto-aof-rewrite-percentage**：设置AOF文件自动重写的触发百分比 **默认值**：100

**auto-aof-rewrite-min-size**：设置AOF文件自动重写的最小文件大小。**默认值**：64MB

> [!CAUTION]
>
> 当AOF文件的大小比上一次重写后的文件大小增长了指定的百分比时，Redis会自动触发AOF文件的重写操作。这有助于控制AOF文件的大小，避免其过度膨胀。
>
> 只有当AOF文件的大小同时满足`auto-aof-rewrite-percentage`和`auto-aof-rewrite-min-size`这两个条件时，Redis才会触发AOF文件的自动重写操作。

### AOF的优点

- **数据安全性高**：AOF文件中记录了所有写操作命令，即使Redis服务器出现崩溃或断电等异常情况，也可以通过AOF文件中的命令来恢复数据。
- **实时性**：AOF持久化机制可以实现实时存储，数据丢失的风险更小。
- **可读性强**：AOF文件以文本格式存储，便于人们阅读和理解存储的数据。
- **灵活性**：Redis提供了多种AOF持久化策略，可以根据实际需求进行调整。

### AOF的缺点

- **文件大小问题**：AOF文件的大小会随着时间的增长而增加，可能会占用较多的磁盘空间。虽然Redis提供了AOF文件重写机制来压缩文件体积，但仍需定期关注AOF文件的大小。
- **性能影响**：AOF持久化机制对于写操作的性能影响相对较大。由于每次写操作都需要追加到AOF文件中，会增加磁盘的IO操作，可能会降低Redis服务器的性能。因此，在选择持久化方式时，需要综合考虑业务场景的需求和系统的性能。

### AOF 重写原理

1. 在重写开始前，redis会创建一个“重写子进程”，这个子进程会读取现有的AOF文件，并将其包含的指令进行分析压缩并写入到一个临时文件中。
2. 与此同时，主进程会将新接收到的写指令一边累积到内存缓冲区中，一边继续写入到原有的AOF文件中，这样做是保证原有的AOF文件的可用性，避免在重写过程中出现意外。
3. 当“重写子进程”完成重写工作后，它会给父进程发一个信号，父进程收到信号后就会将内存中缓存的写指令追加到新AOF文件中
4. 当追加结束后，redis就会用新AOF文件来代替旧AOF文件，之后再有新的写指令，就都会追加到新的AOF文件中
5. 重写aof文件的操作，并没有读取旧的aof文件，而是将整个内存中的数据库内容用命令的方式重写了一个新的aof文件，这点和快照有点类似

### AOF 文件异常修复命令

```
# 进入bin 目录
cd /usr/local/bin
#执行命令
redis-check-aof --fix ./redis/dumpfiles/appendonlydir/appendonly.aof.1.incr.aop
```

