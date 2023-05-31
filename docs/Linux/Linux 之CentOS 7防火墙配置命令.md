### CentOS 7防火墙端口配置

CentOS 升级到7之后，无法使用 iptables控制 Linuxs 的端口。Centos 7使用 firewalld 代替了原来的iptables。

#### 防火墙基本命令如下：

**启动FirewallD服务**命令：

```bash
systemctl start firewalld.service #开启服务

systemctl enable firewalld.service #设置开机启动
```

查看防火墙状态：systemctl status firewalld

```
[root@tool tool]# systemctl status firewalld
firewalld.service - firewalld - dynamic firewall daemon
Loaded: loaded (/usr/lib/systemd/system/firewalld.service; enabled; vendor preset: enabled)
Active: active (running) since So 2021-05-16 15:56:56 CEST; 39min ago
Docs: man:firewalld(1)
Main PID: 656 (firewalld)
CGroup: /system.slice/firewalld.service
└─656 /usr/bin/python2 -Es /usr/sbin/firewalld --nofork --nopid

Mai 16 15:56:55 tool systemd[1]: Starting firewalld - dynamic firewall daemon...
Mai 16 15:56:56 tool systemd[1]: Started firewalld - dynamic firewall daemon.
Mai 16 15:56:56 tool firewalld[656]: WARNING: AllowZoneDrifting is enabled. This is considered an ins... now.
Mai 16 16:19:20 tool firewalld[656]: WARNING: AllowZoneDrifting is enabled. This is considered an ins... now.
Mai 16 16:30:10 tool firewalld[656]: WARNING: AllowZoneDrifting is enabled. This is considered an ins... now.
Hint: Some lines were ellipsized, use -l to show in full.
```

防火墙开通端口：firewall-cmd --zone=public --add-port=8080/tcp --permanent

```
[root@tool tool]# firewall-cmd --zone=public --add-port=8080/tcp --permanent
success
```

<u>命令含义：
--zone #作用域
--add-port=80/tcp #添加端口，格式为：端口/通讯协议
--permanent #永久生效，没有此参数重启后失效</u>

重启防火墙：firewall-cmd --reload

```
[root@tool tool]# firewall-cmd --reload
success
```

查询有哪些端口是开启的: firewall-cmd --list-port

```
[root@tool tool]# firewall-cmd --list-port
3306/tcp 8080/tcp
```

防火墙关闭端口：firewall-cmd --zone=public --remove-port=8080/tcp --permanent

```
[root@tool tool]# firewall-cmd --zone=public --remove-port=8080/tcp --permanent
success
```

关闭firewall：

systemctl stop firewalld.service #停止firewall

systemctl disable firewalld.service #禁止firewall开机启动

### FirewallD 常用的命令：

```
firewall-cmd --state ##查看防火墙状态，是否是running

systemctl status firewalld.service ##查看防火墙状态

systemctl start firewalld.service ##启动防火墙

systemctl stop firewalld.service ##临时关闭防火墙

systemctl enable firewalld.service ##设置开机启动防火墙

systemctl disable firewalld.service ##设置禁止开机启动防火墙

firewall-cmd --permanent --query-port=80/tcp ##查看80端口有没开放

firewall-cmd --reload ##重新载入配置，比如添加规则之后，需要执行此命令

firewall-cmd --get-zones ##列出支持的zone

firewall-cmd --get-services ##列出预定义的服务

firewall-cmd --query-service ftp ##查看ftp服务是否放行，返回yes或者no

firewall-cmd --add-service=ftp ##临时开放ftp服务

firewall-cmd --add-service=ftp --permanent ##永久开放ftp服务

firewall-cmd --remove-service=ftp --permanent ##永久移除ftp服务

firewall-cmd --add-port=80/tcp --permanent ##永久添加80端口

firewall-cmd --zone=public --remove-port=80/tcp --permanent ##移除80端口

iptables -L -n ##查看规则，这个命令是和iptables的相同的

man firewall-cmd ##查看帮助

参数含义：

--zone #作用域

--permanent #永久生效，没有此参数重启后失效

```

