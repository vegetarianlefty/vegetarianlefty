### Mapper.xml配置文件介绍

#### SQL 映射文件只有很少的几个顶级元素（按照应被定义的顺序列出）：

1. cache ：该命名空间的缓存配置。
2. cache-ref ：引用其它命名空间的缓存配置。
3. resultMap ： 描述如何从数据库结果集中加载对象，是最复杂也是最强大的元素。
4. sql ： 可被其它语句引用的可重用语句块。
5. insert ：映射插入语句。
6. update ：映射更新语句。
7. delete ：映射删除语句。
8. select ：映射查询语句。

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.veganlefty.mapper.UserMapper">

    <cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>

    <cache-ref namespace="com.someone.application.data.SomeMapper"/>

    <resultMap id="userResultMap" type="User">
        <id property="id" column="user_id" />
        <result property="username" column="user_name"/>
        <result property="password" column="hashed_password"/>
    </resultMap>

    <sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>

    <insert id="insertAuthor" parameterType="domain.blog.Author" flushCache="true" statementType="PREPARED"
            keyProperty="" keyColumn="" useGeneratedKeys="" timeout="20"></insert>

    <update id="updateAuthor" parameterType="domain.blog.Author" flushCache="true" statementType="PREPARED" timeout="20"></update>

    <delete id="deleteAuthor" parameterType="domain.blog.Author" flushCache="true" statementType="PREPARED" timeout="20"></delete>

    <select id="selectPerson" parameterType="int" parameterMap="deprecated" resultType="hashmap" resultMap="personResultMap" flushCache="false"
            useCache="true" timeout="10" fetchSize="256" statementType="PREPARED" resultSetType="FORWARD_ONLY"></select>
</mapper>
```



#### cache 元素

MyBatis 内置了一个强大的事务性查询缓存机制，它可以非常方便地配置和定制。默认情况下，只启用了本地的会话缓存，它仅仅对一个会话中的数据进行缓存。

```
<cache eviction="FIFO" flushInterval="60000" size="512" readOnly="true"/>
```

eviction 清除策略： 默认的清除策略是 LRU。

LRU：最近最少使用，移除最长时间不被使用的对象。          

FIFO：先进先出，按对象进入缓存的顺序来移除它们。          

SOFT：软引用，基于垃圾回收器状态和软引用规则移除对象。          

 WEAK：弱引用，更积极地基于垃圾收集器状态和弱引用规则移除对象。    

 flushInterval：刷新间隔属性可以被设置为任意的正整数，设置的值应该是一个以毫秒为单位的合理时间量。          默认情况是不设置，也就是没有刷新间隔，缓存仅仅会在调用语句时刷新。        

size：引用数目属性可以被设置为任意正整数，要注意欲缓存对象的大小和运行环境中可用的内存资源。默认值是 1024。        

 readOnly：只读属性可以被设置为 true 或 false。只读的缓存会给所有调用者返回缓存对象的相同实例。          因此这些对象不能被修改。这就提供了可观的性能提升。而可读写的缓存会（通过序列化）返回缓存对象的拷贝。          速度上会慢一些，但是更安全，因此默认值是 false。              

#### cache-ref  元素

在多个命名空间中共享相同的缓存配置和实例

```
<cache-ref namespace="com.someone.application.data.SomeMapper"/>
```



#### resultMap 元素

```
<!-- 非常复杂的结果映射 -->
<resultMap id="detailedBlogResultMap" type="Blog">
  <constructor>
    <idArg column="blog_id" javaType="int"/>
  </constructor>
  <result property="title" column="blog_title"/>
  <association property="author" javaType="Author">
    <id property="id" column="author_id"/>
    <result property="username" column="author_username"/>
    <result property="password" column="author_password"/>
    <result property="email" column="author_email"/>
    <result property="bio" column="author_bio"/>
    <result property="favouriteSection" column="author_favourite_section"/>
  </association>
  <collection property="posts" ofType="Post">
    <id property="id" column="post_id"/>
    <result property="subject" column="post_subject"/>
    <association property="author" javaType="Author"/>
    <collection property="comments" ofType="Comment">
      <id property="id" column="comment_id"/>
    </collection>
    <collection property="tags" ofType="Tag" >
      <id property="id" column="tag_id"/>
    </collection>
    <discriminator javaType="int" column="draft">
      <case value="1" resultType="DraftPost"/>
    </discriminator>
  </collection>
</resultMap>
```

id ：当前命名空间中的一个唯一标识，用于标识一个结果映射。

type：类的完全限定名, 或者一个类型别名。

autoMapping：如果设置这个属性，MyBatis 将会为本结果映射开启或者关闭自动映射。这个属性会覆盖全局的属性 autoMappingBehavior。默认值：未设置（unset）。              

#### sql 元素

```
<sql id="userColumns"> ${alias}.id,${alias}.username,${alias}.password </sql>
```

定义可重用的 SQL 代码片段，以便在其它语句中使用。参数可以静态地（在加载的时候）确定下来，并且可以在不同的 include 元素中定义不同的参数值。

####  insert，update 和 delete 元素

```
<insert id="insertAuthor" parameterType="domain.blog.Author" flushCache="true" statementType="PREPARED" keyProperty="" keyColumn="" useGeneratedKeys="" timeout="20"></insert>

<update id="updateAuthor" parameterType="domain.blog.Author" flushCache="true" statementType="PREPARED" timeout="20"></update>

<delete id="deleteAuthor" parameterType="domain.blog.Author" flushCache="true" statementType="PREPARED" timeout="20"></delete>
```

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| `id`               | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType`    | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为                MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| `parameterMap`     | 用于引用外部 parameterMap 的属性，目前已被废弃。请使用行内参数映射和 parameterType 属性。 |
| `flushCache`       | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：（对 insert、update 和 delete 语句）true。 |
| `timeout`          | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `statementType`    | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用                Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `useGeneratedKeys` | （仅适用于 insert 和 update）这会令 MyBatis 使用 JDBC 的 getGeneratedKeys 方法来取出由数据库内部生成的主键（比如：像 MySQL 和 SQL Server 这样的关系型数据库管理系统的自动递增字段），默认值：false。 |
| `keyProperty`      | （仅适用于 insert 和 update）指定能够唯一识别对象的属性，MyBatis 会使用                getGeneratedKeys 的返回值或 insert 语句的 selectKey 子元素设置它的值，默认值：未设置（`unset`）。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `keyColumn`        | （仅适用于 insert 和 update）设置生成键值在表中的列名，在某些数据库（像 PostgreSQL）中，当主键列不是表中的第一列的时候，是必须设置的。如果生成列不止一个，可以用逗号分隔多个属性名称。 |
| `databaseId`       | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带                databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |

#### select 元素

```
<select id="selectPerson" parameterType="int" parameterMap="deprecated" resultType="hashmap" resultMap="personResultMap" flushCache="false" useCache="true" timeout="10" fetchSize="256" statementType="PREPARED" resultSetType="FORWARD_ONLY"></select>
```

| 属性            | 描述                                                         |
| :-------------- | ------------------------------------------------------------ |
| `id`            | 在命名空间中唯一的标识符，可以被用来引用这条语句。           |
| `parameterType` | 将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。 |
| parameterMap    | 用于引用外部 parameterMap 的属性，目前已被废弃。请使用行内参数映射和 parameterType 属性。 |
| `resultType`    | 期望从这条语句中返回结果的类全限定名或别名。注意，如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身的类型。 resultType 和 resultMap 之间只能同时使用一个。 |
| `resultMap`     | 对外部 resultMap 的命名引用。结果映射是 MyBatis最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。resultType 和 resultMap 之间只能同时使用一个。 |
| `flushCache`    | 将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false。 |
| `useCache`      | 将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。 |
| `timeout`       | 这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。 |
| `fetchSize`     | 这是一个给驱动的建议值，尝试让驱动程序每次批量返回的结果行数等于这个设置值。                默认值为未设置（unset）（依赖驱动）。 |
| `statementType` | 可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。 |
| `resultSetType` | FORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖数据库驱动）。 |
| `databaseId`    | 如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。 |
| `resultOrdered` | 这个设置仅针对嵌套结果 select 语句：如果为 true，则假设结果集以正确顺序（排序后）执行映射，当返回新的主结果行时，将不再发生对以前结果行的引用。这样可以减少内存消耗。默认值：`false`。 |
| `resultSets`    | 这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔。 |