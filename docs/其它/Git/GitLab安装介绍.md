官方网站：https://about.gitlab.com/

安装所需最小配置

内存至少4G

https://docs.gitlab.cn/jh/install/requirements.html

### 在ssh下安装

官方安装文档：https://gitlab.cn/install/?version=ce

#### 1 安装依赖

```
sudo yum install -y curl policycoreutils-python openssh-server perl
sudo systemctl enable sshd
sudo systemctl start sshd
```

#### 2 配置镜像

```
curl -fsSL https://packages.gitlab.cn/repository/raw/scripts/setup.sh | /bin/bash
```

#### 3 开始安装

```
sudo EXTERNAL_URL="http://192.168.44.103" yum install -y gitlab-jh
```

除非您在安装过程中指定了自定义密码，否则将随机生成一个密码并存储在 `/etc/gitlab/initial_root_password` 文件中(出于安全原因，24 小时后，此文件会被第一次 `gitlab-ctl reconfigure` 自动删除，因此若使用随机密码登录，建议安装成功初始登录成功之后，立即修改初始密码）。使用此密码和用户名 `root` 登录。

#### gitlab常用命令

```
gitlab-ctl start                  # 启动所有 gitlab 组件；
gitlab-ctl stop                   # 停止所有 gitlab 组件；
gitlab-ctl restart                # 重启所有 gitlab 组件；
gitlab-ctl status                 # 查看服务状态；
gitlab-ctl reconfigure            # 启动服务；
vi /etc/gitlab/gitlab.rb         # 修改默认的配置文件；
gitlab-ctl tail                   # 查看日志；

```

### 在docker下安装

https://docs.gitlab.cn/jh/install/docker.html

安装所需最小配置

- 内存至少4G
- 系统内核至少在3.10以上 `uname -r` 命令可查看系统内核版本

#### 安装docker

1. 更新yum源

​	`yum update`

2. 安装依赖

​	`yum install -y yum-utils device-mapper-persistent-data lvm2`

3. 添加镜像

```
//国外镜像
yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
//阿里镜像
https://mirrors.aliyun.com/docker-ce/linux/centos/gpg
yum-config-manager --add-repo http://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
```

4. 查看源中可使用版本

```
 yum list docker-ce --showduplicates | sort -r
```

5. 安装指定版本

```
yum install docker
```

6. 配置开机启动项

```
systemctl start docker
systemctl enable docker
docker version
```

#### 使用容器安装gitlab

1.添加容器

```shell
docker run --detach \
  --hostname 192.168.44.103 \
  --publish 443:443 --publish 80:80 \
  --name gitlab \
  --restart always \
  --volume $GITLAB_HOME/config:/etc/gitlab:Z \
  --volume $GITLAB_HOME/logs:/var/log/gitlab:Z \
  --volume $GITLAB_HOME/data:/var/opt/gitlab:Z \
  --shm-size 256m \
  registry.gitlab.cn/omnibus/gitlab-jh:latest
```

2.启动容器

```shell
docker start gitlab
```



3.查看已存在的容器

```shell
docker ps -a
```

4.进入容器

```
docker exec -it  gitlab /bin/bash
```

#### 访问

http://192.168.44.101

当首次运行出现502错误的时候排查两个原因

1. 虚拟机内存至少需要4g
2. 稍微再等等刷新一下可能就好了



#### 管理员账号登录

用户名：root

密码存在下面文件中，登录后需要改密码不然24小时之后会失效

```
cat /etc/gitlab/initial_root_password
```

