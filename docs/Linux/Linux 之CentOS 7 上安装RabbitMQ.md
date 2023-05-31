# 需要安装的文件

https://www.cxyzjd.com/article/weixin_43683536/107187631

启动停止服务 

启动服务 `service rabbitmq-server start` 

关闭服务 `service rabbitmq-server stop` 

查看服务状态 `service rabbitmq-server status` 

重启服务 `service rabbitmq-server restart`

启动服务
 `service rabbitmq-server start`
 启动插件页面管理
 `rabbitmq-plugins enable rabbitmq_management`

创建用户
 `rabbitmqctl add_user admin admin`
 创建用户
 `rabbitmqctl set_user_tags admin administrator`
 赋予权限
 `rabbitmqctl set_permissions -p / admin ".*" ".*" ".*"`

卸载

```
service rabbitmq-server stop 
yum list rabbitmq-server 
yum remove rabbitmq-server 
yum list socat 
yum remove socat 
yum list erlang 
yum remove erlang
```