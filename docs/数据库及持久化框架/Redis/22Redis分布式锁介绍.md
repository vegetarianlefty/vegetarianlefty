### Redis分布式锁介绍

Redis分布式锁是一种用于在多个进程或机器间同步对共享资源访问的机制，以确保在同一时间段内，只有一个进程能获取到锁并访问资源。在分布式系统中，由于存在多个节点，传统的锁机制无法满足需求，因此需要使用分布式锁来控制对共享资源的访问。

### 原理

Redis分布式锁的原理主要是通过Redis的事务来保证锁的互斥和一致性。Redis事务提供了一种将多个命令打包成一个事务、然后一次性执行的能力。利用Redis的SETNX（SET if Not  Exists）命令来判断某个key是否存在，如果不存在则返回1并设置key的值，如果key已经存在则返回0。因此，可以利用SETNX命令来实现获取锁的过程。

### 实现原理

Redis实现分布式锁的具体步骤如下：

1. **尝试获取锁**：在Redis中，使用SETNX命令尝试设置一个不存在的key。如果key存在，则说明锁已经被其他客户端获取；如果key不存在，则设置key的值，并返回1，表示获取锁成功。
2. **设置锁的超时时间**：为了防止锁死，需要为锁设置一个超时时间。当锁的持有者在一段时间后还未释放锁，那么锁就会被强制释放。这可以通过Redis的EXPIRE命令或SET命令的扩展参数来实现。
3. **释放锁**：当持有锁的客户端完成任务后，需要使用DEL命令将锁的key从Redis中删除，从而释放锁。

在实际应用中，为了确保获取锁和设置过期时间的原子性，通常会使用SET命令的扩展参数，如“SET key value NX PX expireTime”，这样可以在一条命令中完成设置键值和设置过期时间的操作。

另外，为了确保释放锁的安全性，只有持有锁的客户端才能释放锁。这可以通过在释放锁之前检查锁的value值来实现。如果value值与持有锁的客户端设置的value值相同，则允许释放锁；否则，拒绝释放锁。这可以通过Lua脚本来实现，以确保操作的原子性。

### 必要条件

1. **互斥性**：这是分布式锁最基本的条件。在分布式系统中，同一时刻只有一个客户端（或进程、线程）能够获取到锁，从而确保对共享资源的独占访问。Redis通过其数据结构和原子操作（如SETNX）来实现这一点。
2. **避免死锁**：为了避免锁因为异常或故障而永久不被释放，导致死锁，分布式锁需要设置过期时间。当锁持有者未能及时释放锁时，锁将在过期时间后自动释放，从而避免死锁的发生。Redis提供了EXPIRE或SET命令的EX选项来设置锁的过期时间。
3. **容错性**：在分布式系统中，节点故障是常见的情况。因此，分布式锁需要具备容错性，能够在节点故障时仍然保证锁的正确性和可用性。例如，当Redis主节点故障时，从节点能够切换为主节点并继续提供锁服务。为了实现这一点，可以使用Redis的哨兵模式或RedLock等算法。
4. **解锁验证**：解锁时需要进行验证，确保只有锁的持有者才能释放锁。这通常通过在设置锁时保存一个唯一的标识（如线程ID或客户端ID），并在解锁时检查该标识来实现。Redis可以通过Lua脚本等原子操作来确保解锁的验证过程。
5. **可重入性**：可重入锁允许同一个线程或进程在已经持有锁的情况下再次获取锁，而不会导致死锁。这是因为在某些情况下，线程或进程可能需要在持有锁的情况下调用其他需要相同锁的方法或函数。Redis分布式锁可以通过记录锁的持有者信息和递归计数来实现可重入性。
6. **高性能和高可用性**：分布式锁需要支持高并发和高性能的获取锁与释放锁操作，同时保证较高的可用性。Redis作为内存数据库，具有高性能和低延迟的特点，适合用于实现分布式锁。此外，通过配置Redis集群和哨兵模式等，可以进一步提高分布式锁的高可用性。

### 注意事项

在使用Redis分布式锁时，需要注意以下几点：

1. **锁的过期时间设置**：要合理设置锁的过期时间，避免锁因持有进程崩溃或网络延迟等原因无法释放，导致死锁。
2. **原子性操作**：获取锁和设置过期时间的操作需要保证原子性，防止出现获取到锁但未成功设置过期时间的情况。
3. **锁的唯一性**：确保生成的锁标识具有唯一性，以避免不同的客户端误释放彼此的锁。
4. **释放锁的安全性**：只有持有锁的客户端才能释放锁，避免误释放。
5. **并发竞争**：在高并发场景下，可能会出现多个客户端同时竞争锁的情况。需要做好并发控制，如使用重试机制等。
6. **网络延迟和故障**：考虑网络延迟和可能的网络故障对锁操作的影响，做好相应的重试和容错机制。
7. **锁的续租**：如果业务处理时间可能超过锁的超时时间，可以在业务进行中对锁进行续租，延长锁的有效时间。
8. **监控和告警**：对Redis服务器的关键指标进行监控，如内存使用、CPU负载、连接数等。当出现异常情况时，及时发送告警通知管理员进行处理。

```
public class RedisDistributedLock{
        private final String script = "if redis.call(\"get\",KEYS[1]) == ARGV[1] then\n" +
            "    return redis.call(\"del\",KEYS[1])\n" +
            "else\n" +
            "    return 0\n" +
            "end";
            
            
     public void redisLock(){
      String uuid = UUID.randomUUID().toString();
        ValueOperations<String, String> ops = stringRedisTemplate.opsForValue();
        String lockKey = "addRedisLock" + "xxxxxxx";
        //防止死锁,并执行业务逻辑，设置 key 并设置过期时间，保证原子性，防止宕机导致锁无法释放
        Boolean lock = ops.setIfAbsent(lockKey, uuid, 5, TimeUnit.SECONDS);
        if (lock) {
        String lockValue = ops.get(lockKey);

            stringRedisTemplate.execute(new DefaultRedisScript<Long>(script, Long.class), Arrays.asList(lockKey), lockValue);
        } else {
            try {
                Thread.sleep(100);
            } catch (InterruptedException e) {
                e.printStackTrace();
            }
             return redisLock();
     }
}
```



### Lua 脚本  加锁和解锁 续期锁

Redis 调用 Lua 脚本通过 eval 命令保证代码执行的原子性，直接用 return 返回脚本执行后的结果



#### 加锁 设计思路

先判断 Redis 分布式锁这个key 是否存在 通过命令 EXISTS key, 

返回零说明不存在，新建当前线程属于自己的锁 BY UUID:ThreadID。

返回壹说明己经有锁，需进一步判断是不是当前线程自己的, 通过命令 HEXISTS key uuid:ThreadID

返回零说明不是自己的，返回壹说明是自己的锁，自增1次表示重入



```
if redis.call('exists',KEYS[1]) == 0 or redis.call('hexists',KEYS[1],ARGV[1]) == 1 then 

  redis.call('hincrby',KEYS[1],ARGV[1],1) 

  redis.call('expire',KEYS[1],ARGV[2]) 

  return 1 

else

  return 0

end
```

#### 解锁 设计思路

先判断 Redis 有锁且还是自己的锁，通过命令 HEXISTS key uuid: ThreadID

返回零，说明根本没有锁，程序块返回nil

不是零，说明有锁且是自己的锁，直接调用HINCRBY 负一表示每次减个一，解锁一次。

直到它变为零表示可以删除该锁Key，del锁key

```
if redis.call('HEXISTS',KEYS[1],ARGV[1]) == 0 then

 return nil

elseif redis.call('HINCRBY',KEYS[1],ARGV[1],-1) == 0 then

 return redis.call('del',KEYS[1])

else

 return 0

end
```

#### 续期锁 设计思路

```
//==============自动续期
if redis.call('HEXISTS',KEYS[1],ARGV[1]) == 1 then
  return redis.call('expire',KEYS[1],ARGV[2])
else
  return 0
end
```

