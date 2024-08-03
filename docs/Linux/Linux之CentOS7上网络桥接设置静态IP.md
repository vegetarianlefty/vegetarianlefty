### 虚拟机设置为桥接模式

在编辑--虚拟网络编辑器中打开编辑器，选择VMnet0，在下面的类型选择桥接，即bridge，网卡不要选择自动（automatic），桥接到后面选择真实的网卡，然后点击应用，确定。

![img](http://image.wangxiaohuan.com/blog/image/202301281034193.png)

![img](http://image.wangxiaohuan.com/blog/image/202301281052656)



在 CentOS7 上查看配置文件，查看路径/etc/sysconfig/network-scripts，修改ifcfg-eth(xxx)文件，如果没有需要新建

![image-20230128105708768](http://image.wangxiaohuan.com/blog/image/202301281057330.png)

（如果没有需要新建，UUID不知道先空着，查看UUID 命令nmcli con show）

```
cd /etc/sysconfig/network-scripts
vim ifcfg-ens192
```



```
#填入内容如下
TYPE=Ethernet   # 网卡类型：为以太网
PROXY_METHOD=none  # 代理方式：关闭状态
BROWSER_ONLY=no  # 只是浏览器：否
BOOTPROTO="static"        #静态IP设置
DEFROUTE=yes   # 默认路由：是
IPV4_FAILURE_FATAL=no # 是不开启IPV4致命错误检测：否
IPV6INIT=yes  # IPV6是否自动初始化: 是[不会有任何影响, 现在还没用到IPV6]
IPV6_AUTOCONF=yes  # IPV6是否自动配置：是[不会有任何影响, 现在还没用到IPV6]
IPV6_DEFROUTE=yes  IPV6是否可以为默认路由：是[不会有任何影响, 现在还没用到IPV6]
IPV6_FAILURE_FATAL=no  # 是不开启IPV6致命错误检测：否
IPV6_ADDR_GEN_MODE=stable-privacy  # IPV6地址生成模型：stable-privacy [这只一种生成IPV6的策略]
NAME=ens192  # 网卡物理设备名称
UUID=a8dc53db-59e4-3497-9c10-dbf01342a50e  # 通用唯一识别码, 每一个网卡都会有, 不能重复,
DEVICE=ens192  # 网卡设备名称, 必须和 `NAME` 值一样
ONBOOT=yes  # 是否开机启动

IPADDR=192.168.1.188   #静态ip地址 与主机中IP的前三位一致，最后一位需要修改，不要跟主机ip一样导致发生冲突
NETMASK=255.255.255.0  #子网掩码 和物理主机一样就可以了
GATEWAY=192.168.1.1   #默认网关 和物理主机一样就可以了
DNS1=114.114.114.114   #DNS
DNS2=8.8.8.8  #DNS

```

```
#重启网络服务
systemctl restart network
```

