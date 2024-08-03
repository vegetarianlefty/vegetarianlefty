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

yum install -y mysql mysql-server

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

