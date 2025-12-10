Redis 批量插入数据

 生成100W条redis批量设置kv的语句(key=kn,value=vn)写入到/tmp目录下的redisTest.txt文件中

```
for((i=1;i<=500*10000;i++)); do echo "set k$i v$i" >> /tmp/redisTest.txt ;done;
```

通过 Redis 提供的管道 --pipe 命令插入生成的批量数据。结合机器 IP 192.168.1.201   端口6380  密码 123456

cat /tmp/redisTest.txt | redis-cli -h 192.168.1.201 -p 6380 -a 123456 --pipe

