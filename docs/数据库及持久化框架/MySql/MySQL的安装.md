# MySQL 安装

所有平台的 MySQL 下载地址为： [MySQL 下载](https://dev.mysql.com/downloads/mysql/) 。  挑选你需要的 *MySQL Community Server* 版本及对应的平台。

> **注意：**安装过程我们需要通过开启管理员权限来安装，否则会由于权限不足导致无法安装。

------

## Linux/UNIX 上安装 MySQL

Linux平台上推荐使用RPM包来安装Mysql,MySQL AB提供了以下RPM包的下载地址：

- **MySQL** - MySQL服务器。你需要该选项，除非你只想连接运行在另一台机器上的MySQL服务器。
- **MySQL-client** - MySQL 客户端程序，用于连接并操作Mysql服务器。
- **MySQL-devel** - 库和包含文件，如果你想要编译其它MySQL客户端，例如Perl模块，则需要安装该RPM包。
- **MySQL-shared** - 该软件包包含某些语言和应用程序需要动态装载的共享库(libmysqlclient.so*)，使用MySQL。
- **MySQL-bench** - MySQL数据库服务器的基准和性能测试工具。

安装前，我们可以检测系统是否自带安装 MySQL:

```
rpm -qa | grep mysql
```

如果你系统有安装，那可以选择进行卸载:

```
rpm -e mysql　　// 普通删除模式
rpm -e --nodeps mysql　　// 强力删除模式，如果使用上面命令删除时，提示有依赖的其它文件，则用该命令可以对其进行强力删除
```

**安装 MySQL：**

接下来我们在 Centos7 系统下使用 yum 命令安装 MySQL，需要注意的是 CentOS 7 版本中 MySQL数据库已从默认的程序列表中移除，所以在安装前我们需要先去官网下载 Yum 资源包，下载地址为：https://dev.mysql.com/downloads/repo/yum/

![img](https://www.runoob.com/wp-content/uploads/2014/03/repo-name-small.png)

```
wget http://repo.mysql.com/mysql80-community-release-el7-7.noarch.rpm

rpm -ivh mysql80-community-release-el7-7.noarch.rpm

yum install -y mysql mysql-server --nogpgcheck

# 修改配置文件
vim /etc/my.cnf
lower_case_table_names=1

# 重启mysql
systemctl restart mysqld

# 查看默认密码
grep password /var/log/mysqld.log

假如默认密码是 abcdef

mysql -u root -p 

# mysql client 链接 mysql
alter user 'root'@'localhost' identified by 'abcdef';
set global validate_password.check_user_name=0;
set global validate_password.policy=0;
set global validate_password.length=1;
//把默认密码改为 root123456
alter user 'root'@'localhost' identified by 'root123456';

# 修改为允许远程访问
use mysql;
update user set host = '%' where user = 'root';
FLUSH PRIVILEGES;
```

> [!NOTE]
>
>  [获取 GPG 密钥失败： [Errno 14\] curl#37 - "Couldn't open file /etc/pki/rpm-gpg/RPM-GPG-KEY-mysql"
>
> 原因： GPG对于包的源key的验证没有通过
>
> 处理办法：
> 在yum install 版本后面加上 --nogpgcheck，即可绕过GPG验证成功安装
> yum install mysql-community-server --nogpgcheck

 MySQL 命令

```
查看root临时密码
grep "A temporary password" /var/log/mysqld.log

启动 MySQL
systemctl start mysqld

停止 MySQL
systemctl stop mysqld

查看 MySQL 运行状态
systemctl status mysqld

开机启动命令：
systemctl enable mysqld
systemctl daemon-reload
```

防火墙设置

```

查看所有打开的端口
firewall-cmd --zone=public --list-ports 

开放3306端口
firewall-cmd --zone=public --add-port=3306/tcp --permanent

关闭3306端口
firewall-cmd --zone=public --remove-port=3306/tcp --permanent

配置立即生效
firewall-cmd --reload 

重启防火墙      
systemctl restart firewalld.service

```

 

### 配置文件

4 个 vCPU 和 16 GB 内存的 CentOS 7 系统上安装 MySQL 5.7 时，可以根据这些资源来优化 MySQL 的配置文件 `/etc/my.cnf`。以下是一个适合这种硬件配置的 MySQL 配置文件示例，包含关键参数的建议值：

`/etc/my.cnf` 配置文件示例

```
[mysqld]
# 基本设置
user = mysql
port = 3306
bind-address = 0.0.0.0
datadir = /var/lib/mysql
socket = /var/lib/mysql/mysql.sock

# MyISAM 引擎设置
key_buffer_size = 64M
myisam_recover_options = FORCE,BACKUP

# InnoDB 引擎设置
innodb_buffer_pool_size = 12G         # 分配给 InnoDB 缓冲池约占总内存的70-75%
innodb_log_file_size = 1G             # 较大的日志文件可以减少磁盘写入次数
innodb_log_buffer_size = 16M          # 日志缓冲区大小
innodb_flush_log_at_trx_commit = 1    # 事务日志的刷新策略 (1 = 每次事务提交时刷新)
innodb_file_per_table = 1             # 为每个表使用单独的表空间
innodb_flush_method = O_DIRECT        # 使用直接 I/O，避免文件系统缓存的双重缓存
innodb_io_capacity = 2000             # I/O 操作的最大数目，适当调整以匹配磁盘能力

# 查询缓存设置 (注意：MySQL 5.7 默认禁用查询缓存)
query_cache_type = 0                  # 禁用查询缓存以避免潜在的锁争用问题
query_cache_size = 0

# 连接设置
max_connections = 500                 # 最大连接数，根据实际需要调整
max_connect_errors = 100000           # 允许最大连接错误数

# 线程和缓存设置
thread_cache_size = 50                # 缓存线程数，减少线程创建的开销
table_open_cache = 4096               # 打开表缓存，适当增大以提高性能
table_definition_cache = 2048         # 表定义缓存大小

# 临时表设置
tmp_table_size = 256M                 # 内存临时表大小
max_heap_table_size = 256M            # 最大内存临时表大小

# 日志设置
log_error = /var/log/mysql/error.log   # 错误日志文件
slow_query_log = 1                    # 启用慢查询日志
slow_query_log_file = /var/log/mysql/mysql-slow.log
long_query_time = 2                   # 慢查询阈值时间 (秒)

# 二进制日志设置 (如果需要开启二进制日志功能)
server-id = 1                         # 服务器ID, 需唯一
log_bin = /var/lib/mysql/mysql-bin
binlog_format = ROW                   # 二进制日志格式 (ROW 更加可靠)
expire_logs_days = 7                  # 二进制日志保留天数
max_binlog_size = 100M                # 二进制日志的最大大小

# 文件句柄限制
open_files_limit = 65535              # 最大文件句柄数

# 调整内存分配和优化内存使用
join_buffer_size = 256K
sort_buffer_size = 4M
read_buffer_size = 4M
read_rnd_buffer_size = 4M
```

### 参数说明

- **`innodb_buffer_pool_size`**: 这是 MySQL InnoDB 引擎的最重要的参数，决定了用于缓存数据和索引的内存大小。设置为可用内存的 70%-75% 左右，确保剩余内存足够处理操作系统和其他进程的需求。
- **`innodb_log_file_size`**: 较大的日志文件可以减少磁盘写入次数，但在崩溃恢复时可能会稍慢。通常设置为 1G 左右。
- **`max_connections`**: 设定 MySQL 同时连接的最大数量，500 是一个合理的默认值，具体数值取决于你的应用负载。
- **`query_cache_size` 和 `query_cache_type`**: 在 MySQL 5.7 中，查询缓存默认禁用，因为它在高并发情况下可能成为瓶颈。
- **`tmp_table_size` 和 `max_heap_table_size`**: 设置内存中允许的最大临时表大小，建议设置为较大的值以减少磁盘临时表的创建。
- **`log_error` 和 `slow_query_log`**: 记录错误日志和慢查询日志对于调试和优化非常重要。
- **`innodb_flush_log_at_trx_commit`**: 设置为 `1` 时，每次事务提交时都会将日志刷新到磁盘，这保证了数据的持久性但可能影响性能。设置为 `2` 可以在稍微牺牲持久性的情况下提高性能。
- **`innodb_io_capacity`**: 调整以匹配磁盘的 I/O 能力，默认值通常较低，可以根据你的磁盘性能适当增加。
- **`open_files_limit`**: 设置 MySQL 进程能够打开的最大文件数，适用于高并发和大量表的情况。

### 其他优化建议

- **定期分析慢查询日志**: 通过分析慢查询日志，识别并优化性能瓶颈。
- **使用 `MySQLTuner` 工具**: 安装并运行 `MySQLTuner`，获取更具体的优化建议。
- **监控系统资源使用**: 使用 `top`, `htop`, `iostat`, `vmstat` 等工具实时监控 CPU、内存和磁盘 I/O，了解 MySQL 对系统资源的消耗情况。

这个配置是基于典型的负载和资源分配的建议。实际生产环境中，可能需要根据应用负载和性能要求进行进一步的微调。

### MySQL 卸载

#### 停止MySQL服务：

在卸载MySQL之前，需要确保MySQL服务已停止。可以使用以下命令来停止MySQL服务：

```
sudo systemctl stop mysqld
```

执行此命令后，MySQL服务将会停止。

#### 卸载MySQL软件包：

CentOS 7中的MySQL通常是通过yum包管理器安装的。要完全卸载MySQL及其所有相关组件，可以使用以下命令：



```
sudo yum remove mysql mysql-server mysql-client mysql-libs mysql-devel mysql-community-server mysql-community-client mysql-community-common
```

执行该命令后，系统会提示确认卸载操作，输入y并按回车键，即可完成软件包的卸载。

#### 删除数据目录：

MySQL的数据文件通常存储在/var/lib/mysql目录下。如果希望清除所有数据，可以手动删除这个目录：



```
sudo rm -rf /var/lib/mysql
```

请注意，这样会删除所有MySQL数据库及其相关数据，因此在执行此操作前，请确保已经备份了所需的数据。

#### 删除配置文件：

MySQL的配置文件一般位于/etc/my.cnf。可以使用以下命令删除它：

```
sudo rm -f /etc/my.cnf
```

此外，还可以检查/etc/mysql/和/etc/my.cnf.d/目录，删除可能存在的其他配置文件：

```
sudo rm -rf /etc/mysql/

sudo rm -rf /etc/my.cnf.d/
```

#### 清除依赖和无用包：

在完成MySQL的卸载后，系统中可能还残留了一些不再需要的依赖包。可以使用以下命令清除这些无用包：

    sudo yum autoremove
    sudo yum clean all
    sudo yum makecache





