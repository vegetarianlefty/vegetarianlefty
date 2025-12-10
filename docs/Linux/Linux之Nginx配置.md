在CentOS 7上安装Nginx，可以通过yum安装或编译安装两种方式来实现。以下是详细的步骤：

### 一、使用yum安装Nginx

1. **添加yum源**：

   Nginx不在默认的yum源中，因此需要使用额外的yum源。可以选择使用Nginx官网提供的源地址，或者使用epel的方式进行安装。

   - 使用官网提供的源地址：
     1. 下载Nginx的rpm包：`wget http://nginx.org/packages/centos/7/noarch/RPMS/nginx-release-centos-7-0.el7.ngx.noarch.rpm`
     2. 使用rpm命令安装下载的rpm包：`rpm -ivh nginx-release-centos-7-0.el7.ngx.noarch.rpm`
   - 使用epel的方式：
     1. 安装yum-utils：`sudo yum install yum-utils`
     2. 安装epel-release：`yum install epel-release`

2. **安装Nginx**：

   使用yum命令安装Nginx：`yum install nginx`

3. **启动并测试Nginx**：

   - 启动Nginx服务：`systemctl start nginx`
   - 设置Nginx开机自启动：`systemctl enable nginx`
   - 测试Nginx是否安装成功：在浏览器中访问服务器的IP地址，如果看到Nginx的欢迎页面，则说明安装成功。

4. **其他操作**：

   - 停止Nginx服务：`systemctl stop nginx`
   - 重启Nginx服务：`systemctl restart nginx`
   - 重新加载Nginx配置：`systemctl reload nginx`

### 二、编译安装Nginx

1. **安装依赖包**：

   编译安装Nginx需要安装一些依赖包，包括gcc、pcre、pcre-devel、zlib、zlib-devel、openssl和openssl-devel等。可以使用yum命令来安装这些依赖包：`yum install gcc pcre pcre-devel zlib zlib-devel openssl openssl-devel -y`

2. **下载Nginx源码包**：

   从Nginx官网下载最新的源码包，例如`nginx-1.22.1.tar.gz`。可以使用wget命令来下载：`wget http://nginx.org/download/nginx-1.22.1.tar.gz`

3. **解压源码包**：

   使用tar命令解压下载的源码包：`tar xf nginx-1.22.1.tar.gz`

4. **编译安装Nginx**：

   进入解压后的Nginx源码目录，执行以下命令进行编译和安装：

   ```
   cd nginx-1.22.1
   ./configure --prefix=/usr/local/nginx --with-http_ssl_module --with-http_stub_status_module --with-pcre
   make
   make install
   ```

5. **启动并测试Nginx**：

   - 进入Nginx的安装目录的sbin子目录：`cd /usr/local/nginx/sbin`
   - 启动Nginx服务：`./nginx`
   - 测试Nginx是否安装成功：在浏览器中访问服务器的IP地址，如果看到Nginx的欢迎页面，则说明安装成功。

6. **其他操作**：

   - 停止Nginx服务：`./nginx -s stop`
   - 重启Nginx服务：`./nginx -s reload`
   - 检查Nginx配置文件是否正确：`./nginx -t`

### 注意事项

1. 在安装Nginx之前，请确保已经安装了必要的依赖包。
2. 在启动Nginx服务之前，请确保防火墙已经开放了Nginx的默认端口（80端口）。
3. 如果需要修改Nginx的默认配置，可以编辑Nginx的配置文件（通常位于`/usr/local/nginx/conf/nginx.conf`或`/etc/nginx/nginx.conf`，具体取决于安装方式和配置）。

通过以上步骤，您可以在CentOS 7上成功安装并运行Nginx。