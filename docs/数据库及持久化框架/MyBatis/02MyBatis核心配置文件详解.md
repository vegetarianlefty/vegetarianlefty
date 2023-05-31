### MyBatis的核心配置文件



核心配置文件主要用于配置连接数据库的环境以及MyBatis的全局配置信息。习惯上命名为mybatis-config.xml。核心配置文件存放的位置是src/main/resources目录下

```
核心配置文件中的标签必须按照固定的顺序:
properties?,settings?,typeAliases?,typeHandlers?,objectFactory?,objectWrapperFactory?,reflectorF actory?,plugins?,environments?,databaseIdProvider?,mappers?
```

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//MyBatis.org//DTD Config 3.0//EN"
        "http://MyBatis.org/dtd/MyBatis-3-config.dtd">
<configuration>
    <!--引入properties文件，此时就可以${属性名}的方式访问属性值-->
    <properties resource="jdbc.properties"></properties>
    <settings>
        <!--将表中字段的下划线自动转换为驼峰-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!--开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
    </settings>
    <typeAliases>
        <!--
        typeAlias:设置某个具体的类型的别名
            属性:
                type:需要设置别名的类型的全类名 alias:设置此类型的别名，若不设置此属性，该类型拥有默认的别名，即类名且不区分大小
                若设置此属性，此时该类型的别名只能使用alias所设置的值 -->
                <!--<typeAlias type="com.mybatis.bean.User"></typeAlias>-->
                <!--<typeAlias type="com.mybatis.bean.User" alias="abc">
        </typeAlias>-->
        <!--以包为单位，设置改包下所有的类型都拥有默认的别名，即类名且不区分大小写-->
        <package name="com.mybatis.bean"/>
    </typeAliases>
    <!-- environments:设置多个连接数据库的环境 属性:
    default:设置默认使用的环境的id
    -->
    <environments default="mysql_test">
        <!--
        environment:设置具体的连接数据库的环境信息
        属性: id:设置环境的唯一标识，可通过environments标签中的default设置某一个环境的id，
        表示默认使用的环境
        -->
        <environment id="mysql_test">
            <!--
            transactionManager:设置事务管理方式
            属性:
            type:设置事务管理方式，type="JDBC|MANAGED" 
                type="JDBC":设置当前环境的事务管理都必须手动处理 
                type="MANAGED":设置事务被管理，例如spring中的AOP
            -->
            <transactionManager type="JDBC"/>
            <!--
            dataSource:设置数据源
            属性:
            type:设置数据源的类型，type="POOLED|UNPOOLED|JNDI"
                type="POOLED":使用数据库连接池，即会将创建的连接进行缓存，下次使用可以从缓存中直接获取，不需要重新创建
                type="UNPOOLED":不使用数据库连接池，即每次使用连接都需要重新创建
                type="JNDI":调用上下文中的数据源 -->
            <dataSource type="POOLED">
                <!--设置驱动类的全类名-->
                <property name="driver" value="${jdbc.driver}"/> <!--设置连接数据库的连接地址-->
                <property name="url" value="${jdbc.url}"/> <!--设置连接数据库的用户名-->
                <property name="username" value="${jdbc.username}"/> <!--设置连接数据库的密码-->
                <property name="password" value="${jdbc.password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--引入映射文件--> <mappers>
    <mapper resource="UserMapper.xml"/>
        <!--
        以包为单位，将包下所有的映射文件引入核心配置文件
        注意:此方式必须保证mapper接口和mapper映射文件必须在相同的包下 -->
        <package name="com.mybatis.mapper"/>
    </mappers>
</configuration>
```



### 创建mapper接口



MyBatis中的mapper接口相当于以前的dao。但是区别在于，mapper仅仅是接口，我们不需要

提供实现类。



### 创建MyBatis的映射文件

相关概念:ORM(Object Relationship Mapping)对象关系映射。

对象:Java的实体类对象 

关系:关系型数据库 

映射:二者之间的对应关系

#### 映射文件的命名规则:

 表所对应的实体类的类名+Mapper.xml 。

例如:表t_user，映射的实体类为User，所对应的映射文件为UserMapper.xml 。

因此一个映射文件对应一个实体类，对应一张表的操作。

 MyBatis映射文件用于编写SQL，访问以及操作表中的数据。

 MyBatis映射文件存放的位置是src/main/resources/mappers目录下。

#### MyBatis中可以面向接口操作数据，要保证两个一致: 

mapper接口的全类名和映射文件的命名空间(namespace)保持一致 。

mapper接口中方法的方法名和映射文件中编写SQL的标签的id属性保持一致。

#### 查询的标签select必须设置属性

用于设置实体类和数据库表的映射关系，主要值为一下

```
<select
  id="selectPerson"
  parameterType="int"
  parameterMap="deprecated"
  resultType="hashmap"
  resultMap="personResultMap"
  flushCache="false"
  useCache="true"
  timeout="10"
  fetchSize="256"
  statementType="PREPARED"
  resultSetType="FORWARD_ONLY">
```

- resultType : 自动映射，用于属性名和表中字段名一致的情况 ，期望从这条语句中返回结果的类全限定名或别名, resultType 和 resultMap 之间只能同时使用一个。

- resultMap : 自定义映射，对外部 resultMap 的命名引用，用于一对多或多对一或字段名和属性名不一致的情况,resultType 和 resultMap 之间只能同时使用一个。
- resultSets :   这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔。    
- ​          





