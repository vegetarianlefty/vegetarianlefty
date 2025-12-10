### Redis 安装

> Linux环境安装Redis必须先具备gcc编译环境,gcc是linux下的一个编译程序，是C程序的编译工具。
>
> 查看 gcc 版本，gcc -v,安装Redis之前需要具备c++库环境.
>
> 安装命令
>
>  sudo yum -y install gcc-c++ automake autoconf libtool make
>
> sudo yum install -y python3
>
> sudo yum install epel-release  # 如果尚未安装EPEL仓库  
>
> sudo yum install jemalloc-devel  # 尝试安装jemalloc开发包

1. 下载获得redis-7.0.0.tar.gz后将它放入我们的Linux目录/opt，下载命令 ：wget https://download.redis.io/releases/redis-7.0.0.tar.gz

2. /opt目录下解压redis，命令：  tar -zxvf redis-7.0.0.tar.gz

3. 进入  redis-7.0.0 目录，进行安装命令： make && make install

4. 将默认的redis.conf 配置文件拷贝到 Redis 自定义目录

5. 修改redis.conf配置文件，

6. 默认daemonize no  改为 daemonize yes， 

7. 默认protected-mode yes   改为 protected-mode no， 

8. 默认bind 127.0.0.1       改为 直接注释掉(默认bind 127.0.0.1只能本机访问)或改成本机IP地址，否则影响远程IP连接，

9. 添加redis密码   改为 requirepass 你自己设置的密码。改完后确保生效，记得重启

10. 进入安装目录，/usr/local/bin目录下运行redis-server，启用自定义目录下的redis.conf文件

   ```
   redis-server /usr/local/bin/redis.conf
   ```

11. 关闭服务 单实例关闭：redis-cli -a 密码 shutdown，多实例关闭，指定端口关闭:redis-cli -p 6379 shutdown

> Redis 卸载，关闭redis-server 服务，删除/usr/local/lib目录下与redis相关的文件
>
> ls -l /usr/local/bin/redis-*
>
> rm -rf /usr/local/bin/redis-*
>
> 查看所有打开的端口
> firewall-cmd --zone=public --list-ports
>
> 打开端口
>
> firewall-cmd --zone=public --add-port=6380/tcp --permanent
>
> 配置立即生效
> firewall-cmd --reload 
>
> 重启防火墙      
> systemctl restart firewalld.service

