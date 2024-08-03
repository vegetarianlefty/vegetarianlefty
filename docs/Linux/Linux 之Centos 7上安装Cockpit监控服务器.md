### Cockpit简介



`Cockpit`是`CentOS 8`内置的一款基于Web的可视化管理工具，对一些常见的命令行管理操作都有界面支持，比如用户管理、防火墙管理、服务器资源监控等，使用非常方便，号称人人可用的Linux管理工具。



### Cockpit安装启动



- `CentOS 8`默认已安装Cockpit，直接启动服务即可；

```
# 配置cockpit服务开机自启
systemctl enable --now cockpit.socket
# 启动cockpit服务
systemctl start cockpit
```

- `CentOS 7`上如果要使用Cockpit的话，需要自行安装，并开放对应服务；

```
# 安装
yum install cockpit
# 启用服务
systemctl enable --now cockpit.socket
# 开放服务
firewall-cmd --permanent --zone=public --add-service=cockpit
# 重新加载防护墙
firewall-cmd --reload
```

- 安装完成后即可通过浏览器访问Cockpit，使用Linux用户即可登录（比如root用户），访问地址：http://192.168.1.111:9090/（服务器IP地址）