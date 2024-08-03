### 基础知识

####  主从复制原理

MySQL 的主从复制是依赖于 binlog，也就是记录 MySQL 上的所有变化并以二进制形式保存在磁盘上二进制日志文件。

主从复制就是将 binlog 中的数据从主库传输到从库上，一般这个过程是异步的，即主库上的操作不会等待 binlog 同步地完成。

![图片](http://image.wangxiaohuan.com/blog/image/202311221052462.png)

详细流程如下：

1. 主库写 binlog：主库的更新 SQL(update、insert、delete) 被写到 binlog；
2. 主库发送 binlog：主库创建一个 log dump 线程来发送 binlog 给从库；
3. 从库写 relay log：从库在连接到主节点时会创建一个 IO 线程，以请求主库更新的 binlog，并且把接收到的 binlog 信息写入一个叫做 relay log 的日志文件；
4. 从库回放：从库还会创建一个 SQL 线程读取 relay log 中的内容，并且在从库中做回放，最终实现主从的一致性。

####  Cannel 基础

Canel 是一款常用的数据同步工具，其原理是基于 Binlog 订阅的方式实现，**模拟一个 MySQL Slave 订阅 Binlog 日志，从而实现 CDC**（Change Data Capture），将已提交的更改发送到下游。

主要流程如下：

1. Canal 服务端向 MySQL 的 master 节点传输 dump 协议；
2. MySQL 的 master 节点接收到 dump 请求后推送 Binlog 日志给 Canal 服务端，解析 Binlog 对象（原始为 byte 流）转成 Json 格式；
3. Canal 客户端通过 TCP 协议或 MQ 形式监听 Canal 服务端，同步数据到 ES。

![图片](http://image.wangxiaohuan.com/blog/image/202311221052862.png)

下面是 Cannel 执行的核心流程，其中 Binlog Parser 主要负责 Binlog 的提取、解析和推送，EventSink 负责数据的过滤 、路由和加工，仅作了解即可。

![图片](http://image.wangxiaohuan.com/blog/image/202311221052505.png)

### 软件下载安装

#### Java JDK

- 官网：https://www.oracle.com/java/technologies/downloads/
- JDK 版本：11.0.19

由于 Canal 和 ES 的安装，都强依赖 JDK，所以这里有必要先说明。

![图片](http://image.wangxiaohuan.com/blog/image/202311221053257.png)

**前方高能，这里有坑！！！**

如果你选的版本不对，ES 安装可能会失败，然后 Canal 同步数据到 ES 时，也会出现很多诡异的问题。

#### MySQL

MySQL 大家应该都安装了，这里需要打开 MySQL 的 BinLog。

我是 Mac，主要新建一个 my.cnf 文件，然后再重启 MySQL。

![图片](http://image.wangxiaohuan.com/blog/image/202311221053321.png)

![图片](http://image.wangxiaohuan.com/blog/image/202311221053041.png)

这里重启 MySQL，BinLog 开启后，会看到 BinLog 日志。

![图片](http://image.wangxiaohuan.com/blog/image/202311221054224.png)

然后需要创建一个账号，账号和密码都是 Cannal，给后面 Canal 使用。

```
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'localhost' IDENTIFIED BY 'canal' ;
```

####  Canal

- 官网：https://github.com/alibaba/canal/releases
- 版本：v1.1.6

![图片](http://image.wangxiaohuan.com/blog/image/202311221054434.png)

下载 canal.adapter 和 canal.deployer 两个就可以：

- canal.deployer：相当于 canal 的服务端，启动它才可以在客户端接收数据库变更信息。
- canal.adapter：增加客户端数据落地的适配及启动功能(当 deployer 接收到消息后，会根据不同的目标源做适配，比如是 es 目标源适配和 hbase 适配等等)。

备注：canal.admin 为 canal提供整体配置管理、节点运维等面向运维的功能，提供相对友好的 WebUI 操作界面，方便更多用户快速和安全的操作，我这边使用的是单机的，因此就没有下载安装，大家也可以拉 source code 源码去研究下。

####  ES

- ES 官网：https://www.elastic.co/cn/downloads/elasticsearch
- ES 版本：7.17.4

Mac 安装 ES 非常简单：

```
brew install elasticsearch
```

安装细节不赘述，安装成功后，输入以下网址：

```
http://localhost:9200/?pretty
```

![图片](http://image.wangxiaohuan.com/blog/image/202311221054775.png)

#### Kibana

- 下载网址：https://www.elastic.co/cn/downloads/past-releases#kibana
- 版本：7.14.0

![图片](http://image.wangxiaohuan.com/blog/image/202311221054134.png)

它是 ES 的界面化操作工具，安装细节不赘述，安装成功后，输入以下网址：

```
http://localhost:5601/app/dev_tools#/console
```

![图片](http://image.wangxiaohuan.com/blog/image/202311221055209.png)

####  IK 分词器

- 下载网址：https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v7.17.2
- 版本：v7.17.2

它是 ES 的分词器，安装细节不赘述，安装成功后，可以验证一下分词效果。

![图片](http://image.wangxiaohuan.com/blog/image/202311221055408.png)

#### 小节

MySQL 开启 BinLog，这个不难，主要观察是否有 BinLog 日志。

Canal 的安装是最复杂的，涉及到很多配置修改，后面会讲解。

最后是 ES + Kibana + IK 分词器，这个其实也不难，主要关注 ES 绑定的 JDK 版本，三款软件的安装可以参考这篇：https://blog.csdn.net/weixin_46049028/article/details/129956485

###  Canal 配置

####  canal.deployer 配置

修改 conf—>example 文件夹的 instance.properties 监听的数据库配置。

![图片](http://image.wangxiaohuan.com/blog/image/202311221055860.png)

这里主要修改的监听 MySQL 的 URL、用户名和密码。

![图片](http://image.wangxiaohuan.com/blog/image/202311221055616.png)

这里默认的账号密码就是 canal，前面已经教大家如何创建了。

#### canal.deployer 启动

在 canal.deployer 中的 bin 文件下去启动命令 startup.sh

![图片](http://image.wangxiaohuan.com/blog/image/202311221055806.png)

![图片](http://image.wangxiaohuan.com/blog/image/202311221055107.png)

这样就代表已经启动了，我们可以去看下启动日志。

![图片](http://image.wangxiaohuan.com/blog/image/202311221055508.png)

![图片](http://image.wangxiaohuan.com/blog/image/202311221056296.png)

上面 start successful 代表已经启动成功，并且已经监听我的 MySQL 数据库。

#### canal.adapte 配置

**Step1**: 先把 adapter 下面的 bootstrap.yml，全部注释掉，否则会提示你 XX 表不存在，这里坑了我好惨。

![图片](http://image.wangxiaohuan.com/blog/image/202311221056792.png)

![图片](http://image.wangxiaohuan.com/blog/image/202311221056118.png)

**Step2**: 再修改 adapter 的 application.yml 配置文件。

![图片](http://image.wangxiaohuan.com/blog/image/202311221056464.png)

这里的坑，一般就是 mysql 的账号密码不对，或者给的 es 链接，没有"http://"前缀，这些都是我过踩的坑。

**Step3**: 修改我们在 application.yml 中配置的目标数据源 es7 文件夹内容。

![图片](http://image.wangxiaohuan.com/blog/image/202311221056673.png)

由于我们这里是对表 article 进行去监听，因此我们在  es7 文件夹中去创建 article.yml 文件。

![图片](http://image.wangxiaohuan.com/blog/image/202311221056020.png)

由于我们需要把技术派项目中的文章查询功能，改造成 ES 的查询方式，所以我们就把技术派的文章表 article，同步到 ES 中。

yml文件配置如下：

![图片](http://image.wangxiaohuan.com/blog/image/202311221056691.png)

```
dataSourceKey: defaultDS # 源数据源的key, 对应上面配置的srcDataSources中的值
destination: example  # canal的instance或者MQ的topic
groupId: g1 # 对应MQ模式下的groupId, 只会同步对应groupId的数据
esMapping:
  _index: article # es 的索引名称
  _id: _id  # es 的_id, 如果不配置该项必须配置下面的pk项_id则会由es自动分配
  sql: "SELECT t.id AS _id,t.id,t.user_id,t.article_type,t.title,t.short_title,t.picture,
        t.summary,t.category_id,t.source,t.source_url,t.offical_stat,t.topping_stat,
        t.cream_stat,t.`status`,t.deleted,t.create_time,t.update_time
        FROM article t"        # sql映射
  commitBatch: 1   # 提交批大小
```

**Step4**: 在 Kibana 中创建 ES 的 article 索引

代码如下：

```
PUT /article
{
    "mappings" : {
      "properties" : {
        "id" : {
          "type" : "integer"
        },
        "user_id" : {
          "type" : "integer"
        },
        "article_type" : {
          "type" : "integer"
        },
        "title" : {
          "type" : "text",
          "analyzer": "ik_max_word"
        },
        "short_title" : {
          "type" : "text",
          "analyzer": "ik_max_word"
        },
        "picture" : {
          "type" : "text",
          "analyzer": "ik_max_word"
        },
        "summary" : {
          "type" : "text",
          "analyzer": "ik_max_word"
        },
        "category_id" : {
          "type" : "integer"
        },
        "source" : {
          "type" : "integer"
        },
        "source_url" : {
          "type" : "text",
          "analyzer": "ik_max_word"
        },
        "offical_stat" : {
          "type" : "integer"
        },
        "topping_stat" : {
          "type" : "integer"
        },
        "cream_stat" : {
          "type" : "integer"
        },
        "status" : {
          "type" : "integer"
        },
        "deleted" : {
          "type" : "integer"
        },
        "create_time" : {
          "type" : "date"
        },
        "update_time" : {
          "type" : "date"
        }
      }
    }
 }
```

####  canal.adapte 启动

![图片](http://image.wangxiaohuan.com/blog/image/202311221056344.png)

我们看下启动日志：

![图片](http://image.wangxiaohuan.com/blog/image/202311221057933.png)

上面没有任何报错，并且已经启动了 8081 端口，说明已经启动成功，此时我们就可以操作了。

###  数据同步实战

#### 全量同步

在开始 adapter 之后，我们应该先来一把全量数据同步，在源码中提供了一个接口进行全量同步，命令如下：

```
curl http://127.0.0.1:8081/etl/es7/article.yml -X POST
```

![图片](http://image.wangxiaohuan.com/blog/image/202311221057715.png)

上面就是执行同步成功后，提示已经导入 10 条。

![图片](http://image.wangxiaohuan.com/blog/image/202311221057865.png)

#### 增量同步

增量数据就是当我在 MySQL 中 update、delete 和 insert 时，那么 ES 中数据也会对应发生变化