### 逻辑删除实现

**概念:**

逻辑删除，可以方便地实现对数据库记录的逻辑删除而不是物理删除。逻辑删除是指通过更改记录的状态或添加标记字段来模拟删除操作，从而保留了删除前的数据，便于后续的数据分析和恢复。

-   物理删除：真实删除，将对应数据从数据库中删除，之后查询不到此条被删除的数据
-   逻辑删除：假删除，将对应数据中代表是否被删除字段的状态修改为“被删除状态”，之后在数据库中仍旧能看到此条数据记录

**逻辑删除实现:**

1. 数据库和实体类添加逻辑删除字段

   1. 表添加逻辑删除字段

      可以是一个布尔类型、整数类型或枚举类型。

      ```sql
      ALTER TABLE USER ADD deleted INT DEFAULT 0 ;  # int 类型 1 逻辑删除 0 未逻辑删除
      ```

   2. 实体类添加逻辑删除属性

      ```sql
      @Data
      public class User {
      
         // @TableId
          private Integer id;
          private String name;
          private Integer age;
          private String email;
          
          @TableLogic
          //逻辑删除字段 int mybatis-plus下,默认 逻辑删除值为1 未逻辑删除 1 
          private Integer deleted;
      }
      
      ```

2. 指定逻辑删除字段和属性值

   1. 单一指定

      ```sql
      @Data
      public class User {
      
         // @TableId
          private Integer id;
          private String name;
          private Integer age;
          private String email;
           @TableLogic
          //逻辑删除字段 int mybatis-plus下,默认 逻辑删除值为1 未逻辑删除 1 
          private Integer deleted;
      }
      ```

   2. 全局指定

      ```yaml
      mybatis-plus:
        global-config:
          db-config:
            logic-delete-field: deleted # 全局逻辑删除的实体字段名(since 3.3.0,配置后可以忽略不配置步骤2)
            logic-delete-value: 1 # 逻辑已删除值(默认为 1)
            logic-not-delete-value: 0 # 逻辑未删除值(默认为 0)
      ```

3. 演示逻辑删除操作

   > 逻辑删除以后,没有真正的删除语句,删除改为修改语句!
   > 删除代码:

   ```java
   //逻辑删除
   @Test
   public void testQuick5(){
       //逻辑删除
       userMapper.deleteById(5);
   }
   ```

   执行效果:

   JDBC Connection \[com.alibaba.druid.proxy.jdbc.ConnectionProxyImpl\@5871a482] will not be managed by Spring
   \==> Preparing: UPDATE user SET deleted=1 WHERE id=? AND deleted=0
   \==> Parameters: 5(Integer)
   <==    Updates: 1

4. 测试查询数据

   ```java
   @Test
   public void testQuick6(){
       //正常查询.默认查询非逻辑删除数据
       userMapper.selectList(null);
   }
   
   //SELECT id,name,age,email,deleted FROM user WHERE deleted=0
   ```

###  乐观锁实现

####  悲观锁和乐观锁场景和介绍

**并发问题场景演示:**

![](http://image.wangxiaohuan.com/blog/image/202310191123297.png)

\*\*解决思路: \*\*

乐观锁和悲观锁是在并发编程中用于处理并发访问和资源竞争的两种不同的锁机制!!

悲观锁： &#x20;
悲观锁的基本思想是，在整个数据访问过程中，将共享资源锁定，以确保其他线程或进程不能同时访问和修改该资源。悲观锁的核心思想是"先保护，再修改"。在悲观锁的应用中，线程在访问共享资源之前会获取到锁，并在整个操作过程中保持锁的状态，阻塞其他线程的访问。只有当前线程完成操作后，才会释放锁，让其他线程继续操作资源。这种锁机制可以确保资源独占性和数据的一致性，但是在高并发环境下，悲观锁的效率相对较低。

乐观锁： &#x20;
乐观锁的基本思想是，认为并发冲突的概率较低，因此不需要提前加锁，而是在数据更新阶段进行冲突检测和处理。乐观锁的核心思想是"先修改，后校验"。在乐观锁的应用中，线程在读取共享资源时不会加锁，而是记录特定的版本信息。当线程准备更新资源时，会先检查该资源的版本信息是否与之前读取的版本信息一致，如果一致则执行更新操作，否则说明有其他线程修改了该资源，需要进行相应的冲突处理。乐观锁通过避免加锁操作，提高了系统的并发性能和吞吐量，但是在并发冲突较为频繁的情况下，乐观锁会导致较多的冲突处理和重试操作。

理解点: 悲观锁和乐观锁是两种解决并发数据问题的思路,不是具体技术!!!

**具体技术和方案:**

1.  乐观锁实现方案和技术：
    -   版本号/时间戳：为数据添加一个版本号或时间戳字段，每次更新数据时，比较当前版本号或时间戳与期望值是否一致，若一致则更新成功，否则表示数据已被修改，需要进行冲突处理。
    -   CAS（Compare-and-Swap）：使用原子操作比较当前值与旧值是否一致，若一致则进行更新操作，否则重新尝试。
    -   无锁数据结构：采用无锁数据结构，如无锁队列、无锁哈希表等，通过使用原子操作实现并发安全。
2.  悲观锁实现方案和技术：
    -   锁机制：使用传统的锁机制，如互斥锁（Mutex Lock）或读写锁（Read-Write Lock）来保证对共享资源的独占访问。
    -   数据库锁：在数据库层面使用行级锁或表级锁来控制并发访问。
    -   信号量（Semaphore）：使用信号量来限制对资源的并发访问。

**介绍版本号乐观锁技术的实现流程:**

-   每条数据添加一个版本号字段version
-   取出记录时，获取当前 version
-   更新时，检查获取版本号是不是数据库当前最新版本号
-   如果是\[证明没有人修改数据], 执行更新, set 数据更新 , version = version+ 1&#x20;
-   如果 version 不对\[证明有人已经修改了]，我们现在的其他记录就是失效数据!就更新失败

#### 3.2.2 使用mybatis-plus数据使用乐观锁

&#x20;1\.  添加版本号更新插件

```java
@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
    MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
    interceptor.addInnerInterceptor(new OptimisticLockerInnerInterceptor());
    return interceptor;
}
```

1. 乐观锁字段添加@Version注解

   注意: 数据库也需要添加version字段

   ```java
   ALTER TABLE USER ADD VERSION INT DEFAULT 1 ;  # int 类型 乐观锁字段
   ```

   -   支持的数据类型只有:int,Integer,long,Long,Date,Timestamp,LocalDateTime
   -   仅支持 `updateById(id)` 与 `update(entity, wrapper)` 方法

   ```java
   @Version
   private Integer version;
   ```

2. 正常更新使用即可

   ```java
   //演示乐观锁生效场景
   @Test
   public void testQuick7(){
       //步骤1: 先查询,在更新 获取version数据
       //同时查询两条,但是version唯一,最后更新的失败
       User user  = userMapper.selectById(5);
       User user1  = userMapper.selectById(5);
   
       user.setAge(20);
       user1.setAge(30);
   
       userMapper.updateById(user);
       //乐观锁生效,失败!
       userMapper.updateById(user1);
   }
   ```

### 3.3 防全表更新和删除实现

针对 update 和 delete 语句 作用: 阻止恶意的全表更新删除

添加防止全表更新和删除拦截器

```java
@Bean
public MybatisPlusInterceptor mybatisPlusInterceptor() {
  MybatisPlusInterceptor interceptor = new MybatisPlusInterceptor();
  interceptor.addInnerInterceptor(new BlockAttackInnerInterceptor());
  return interceptor;
}
}
```

测试全部更新或者删除

```java
@Test
public void testQuick8(){
    User user = new User();
    user.setName("custom_name");
    user.setEmail("xxx@mail.com");
    //Caused by: com.baomidou.mybatisplus.core.exceptions.MybatisPlusException: Prohibition of table update operation
    //全局更新,报错
    userService.saveOrUpdate(user,null);
}
```

# 