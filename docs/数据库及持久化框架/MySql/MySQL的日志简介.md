### MySQL的日志系统组成

- 重做日志（redo log）
- 回滚日志（undo log）
- 二进制日志（binlog）
- 错误日志（errorlog）
- 慢查询日志（slow query log）
- 一般查询日志（general log）
- 中继日志（relay log）

本文主要介绍重做日志和回滚日志的关系，他们都属于事务操作相关，是InnoDB引擎特有的。二进制日志也与事务有一定的关系，是Server层实现的，所有的引擎都可以使用。

#### redo log 介绍

redo log是重做日志，作用是确保事务的持久性，是物理格式的日志，是InnoDB引擎特有的，它记录的是数据库中每个页的修改。redo log是在事务开始之前就产生了，在事务提交的时候，必须先把该事务的中所有的事务日志写入到磁盘中的redo log中。redo log是逐步写入到磁盘中的，有固定空间大小，超过了之后会被覆盖重写。

#### undo log 介绍

undo log是回滚日志，作用提供回滚操作,用来回滚行记录到某个版本。是逻辑日志，根据每行记录进行记录。事务开始之前，将当前是的版本生成undo log，undo 也会产生 redo 来保证undo log的可靠性。

#### binlog 介绍

binlog是二进制日志，作用是用于复制，在主从复制中，从库利用主库上的binlog进行重播，实现主从同步。用于数据库的基于时间点的还原。是逻辑日志，是执行过的事务中的sql语句。事务提交的时候，一次性将事务中的sql语句（一个事物可能对应多个sql语句）按照一定的格式记录到binlog中。

#### errorlog 介绍

errorlog是错误日志，记录mysql服务的启停时正确和错误的信息，还记录启动、停止、运行过程中的错误信息。

#### slow query log 介绍

slow query log是慢查询日志，记录所有执行时间超过long_query_time的所有查询或不使用索引的查询。

#### general log 介绍

general log是查询日志，记录建立的客户端连接和执行的语句。

#### relay log 介绍

relay log是中继日志，主从复制时使用的日志。