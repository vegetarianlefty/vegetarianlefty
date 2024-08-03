### 开启防火墙

```
firewall-cmd --zone=public --add-port=端口号/tcp --permanent
firewall-cmd --reload  开启端口防火墙
```

### 查看磁盘存储空间

```
[root@master-03 data]# df -h
Filesystem           Size  Used Avail Use% Mounted on
devtmpfs             2.9G     0  2.9G   0% /dev
tmpfs                3.0G     0  3.0G   0% /dev/shm
tmpfs                3.0G  278M  2.7G  10% /run
tmpfs                3.0G     0  3.0G   0% /sys/fs/cgroup
/dev/mapper/cl-root   50G   16G   35G  31% /
/dev/mapper/cl-home  344G  2.5G  341G   1% /home
/dev/sda1            976M  153M  756M  17% /boot
tmpfs                596M     0  596M   0% /run/user/0
```

### 查看系统内存占用

```
[root@master-03 data]# free -m
              total        used        free      shared  buff/cache   available
Mem:           5956        3444         271         277        2239        1945
Swap:             0           0           0
```

### Linux下查看占用磁盘较大的文件

linux查看[根目录](https://so.csdn.net/so/search?q=根目录&spm=1001.2101.3001.7020)下所有文件夹大小的方法如下：
 1、进入根目录：cd /
 2、使用命令 ： du -sh * 查看根目录下每个文件夹的大小
 3、然后再进入占用空间比较大的文件夹，然后再使用2中命令查找大文件，依次类推

### Linux 内网传输文件

- 远端到本地
   scp -rp cccc55@192.168.1.3:/home/path /home/mypath
- 本地到远端
   scp -rp 本地地址 远端地址