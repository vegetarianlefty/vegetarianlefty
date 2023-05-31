### 改表法

```
#1. 输入密码登录mysql，执行第二步之前需要先使用：use mysql来选中mysql这个表！
mysql -u root -p

use mysql  
#2. 查看mysql这个表的内容
select host,user from user where user='root';

#3. 将表内的localhost这个值更新为'%'  
update user set host = '%' where user='root' and host='localhost';

#4. 
select host, user from user where user='root';
```

> Host列表指定了允许用户登录所使用的IP，比如user=root Host=192.168.1.1。这里的意思就是说root用户只能通过192.168.1.1的客户端去访问。
> 而%是个通配符，如果Host=192.168.1.%，那么就表示只要是IP地址前缀为“192.168.1.”的客户端都可以连接。如果Host=%，表示所有IP都有连接权限。

### 授权法

```
所有用户授权
#1. 授权用户访问数据库
GRANT ALL PRIVILEGES ON *.* TO 'user'@'%' IDENTIFIED BY 'password' WITH GRANT OPTION; 

#2.刷新MySQL的系统权限相关表
FLUSH   PRIVILEGES;#注意授权后必须FLUSH PRIVILEGES;否则无法立即生效.

#3.将'user'和'password'字段修改为你的数据库用户名和密码（注意）。

单用户授权
#1.授权用户访问数据库
GRANT ALL PRIVILEGES ON . TO 'user'@'192.168.2.158' IDENTIFIED BY 'password' WITH GRANT OPTION;

#2.刷新MySQL的系统权限相关表
FLUSH PRIVILEGES;

#3.将'user'和'password'字段修改为你的数据库用户名和密码。

基于mysql的分支MariaDB使用授权法连接远程数据库(主要区别是命令使用小写以及user名称不带 ‘ ‘ )

#1.执行命令输入你的密码，进入数据库
mysql -u root -p 

#2.下面命令中root替换为你自己的数据库用户  'password'替换为你自己的密码

grant all privileges on *.* to root@'%' identified by 'password' with grant option;

#3.刷新MySQL的系统权限相关表
flush privileges; 
```