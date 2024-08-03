### frp 是什么？

frp 是一款高性能的反向代理应用，专注于内网穿透。它支持多种协议，包括 TCP、UDP、HTTP、HTTPS 等，并且具备 P2P 通信功能。使用 frp，您可以安全、便捷地将内网服务暴露到公网，通过拥有公网 IP 的节点进行中转。

### 为什么选择 frp？

通过在具有公网 IP 的节点上部署 frp 服务端，您可以轻松地将内网服务穿透到公网，并享受以下专业特性：

- 多种协议支持：客户端服务端通信支持 TCP、QUIC、KCP 和 Websocket 等多种协议。
- TCP 连接流式复用：在单个连接上承载多个请求，减少连接建立时间，降低请求延迟。
- 代理组间的负载均衡。
- 端口复用：多个服务可以通过同一个服务端端口暴露。
- P2P 通信：流量不必经过服务器中转，充分利用带宽资源。
- 客户端插件：提供多个原生支持的客户端插件，如静态文件查看、HTTPS/HTTP 协议转换、HTTP、SOCKS5 代理等，以便满足各种需求。
- 服务端插件系统：高度可扩展的服务端插件系统，便于根据自身需求进行功能扩展。
- 用户友好的 UI 页面：提供服务端和客户端的用户界面，使配置和监控变得更加方便。

### 下载

您可以从 GitHub 的 [Release](https://github.com/fatedier/frp/releases) 页面中下载最新版本的客户端和服务器二进制文件。所有文件都打包在一个压缩包中，还包含了一份完整的配置参数说明。

### 部署

1. 解压下载的压缩包。
2. 将 `frpc` 复制到内网服务所在的机器上。
3. 将 `frps` 复制到拥有公网 IP 地址的机器上，并将它们放在任意目录。

### 开始使用！

1. 编写配置文件，目前支持的文件格式包括 TOML/YAML/JSON，旧的 INI 格式仍然支持，但已经不再推荐。
2. 使用以下命令启动服务器：`./frps -c ./frps.toml`。
3. 使用以下命令启动客户端：`./frpc -c ./frpc.toml`。
4. 如果需要在后台长期运行，建议结合其他工具，如 [systemd](https://gofrp.org/zh-cn/docs/setup/systemd/) 和 `supervisor`。

# 使用 systemd

此示例演示如何在 Linux 系统下使用 systemd 来管理 frps 服务，包括启动、停止、配置后台运行和设置开机自启动。



在 Linux 系统下，使用 `systemd` 可以方便地控制 frps 服务端的启动、停止、配置后台运行以及开机自启动。

以下是具体的操作步骤：

1. **安装 systemd**

   如果您的 Linux 服务器上尚未安装 systemd，可以使用包管理器如 `yum`（适用于 CentOS/RHEL）或 `apt`（适用于 Debian/Ubuntu）来安装它：

   ```bash
   # 使用 yum 安装 systemd（CentOS/RHEL）
   yum install systemd
   
   # 使用 apt 安装 systemd（Debian/Ubuntu）
   apt install systemd
   ```

2. **创建 frps.service 文件**

   使用文本编辑器 (如 vim) 在 `/etc/systemd/system` 目录下创建一个 `frps.service` 文件，用于配置 frps 服务。

   ```bash
   $ sudo vim /etc/systemd/system/frps.service
   ```

   写入内容

   ```ini
   [Unit]
   # 服务名称，可自定义
   Description = frp server
   After = network.target syslog.target
   Wants = network.target
   
   [Service]
   Type = simple
   # 启动frps的命令，需修改为您的frps的安装路径
   ExecStart = /path/to/frps -c /path/to/frps.toml
   # 重启策略
   Restart=always
   [Install]
   WantedBy = multi-user.target
   ```

3. **使用 systemd 命令管理 frps 服务**

   ```bash
   # 重新加载 systemd 管理器配置
   sudo systemctl daemon-reload
   # 启动frp
   sudo systemctl start frps
   # 停止frp
   sudo systemctl stop frps
   # 重启frp
   sudo systemctl restart frps
   # 查看frp状态
   sudo systemctl status frps
   ```

4. **设置 frps 开机自启动**

   ```bash
   sudo systemctl enable frps
   ```

通过遵循上述步骤，您可以轻松地使用 systemd 来管理 frps 服务，实现启动、停止、自动运行和开机自启动。确保替换路径和配置文件名称以匹配您的实际安装。