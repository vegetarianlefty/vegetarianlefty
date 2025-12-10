### 进入yum的文件夹

命令：cd  /etc/yum.repos.d/

```
[vagrant@tool yum.repos.d]$ cd /etc/yum.repos.d ll
[vagrant@tool yum.repos.d]$ ll
insgesamt 24
-rw-r--r--. 1 root root 1664  1. Apr 2015  CentOS-Base.repo
-rw-r--r--. 1 root root 1309  1. Apr 2015  CentOS-CR.repo
-rw-r--r--. 1 root root  649  1. Apr 2015  CentOS-Debuginfo.repo
-rw-r--r--. 1 root root  290  1. Apr 2015  CentOS-fasttrack.repo
-rw-r--r--. 1 root root 1331  1. Apr 2015  CentOS-Sources.repo
-rw-r--r--. 1 root root 1002  1. Apr 2015  CentOS-Vault.repo
```



### 删除yum文件夹所有yum源

命令：rm -rf  /etc/yum.repos.d/*.repo

```
[vagrant@tool yum.repos.d]$ sudo rm -rf  /etc/yum.repos.d/*.repo
[vagrant@tool yum.repos.d]$ ll
insgesamt 0
[vagrant@tool yum.repos.d]$ 
```

### 在 CentOS上安装 wget

```
 # CentOS 7 或 RHEL 7 
sudo yum install wget  
 # CentOS 8 或 RHEL 8 及以上
sudo dnf install wget 
```



### 利用[wget](https://so.csdn.net/so/search?q=wget&spm=1001.2101.3001.7020)下载阿里云repo文件

命令：wget  http://mirrors.aliyun.com/repo/Centos-7.repo

```
[vagrant@tool yum.repos.d]$ sudo wget  http://mirrors.aliyun.com/repo/Centos-7.repo
--2022-02-16 16:10:00--  http://mirrors.aliyun.com/repo/Centos-7.repo
Auflösen des Hostnamen »mirrors.aliyun.com (mirrors.aliyun.com)«... 183.204.203.226, 183.204.203.224, 183.204.203.231, ...
Verbindungsaufbau zu mirrors.aliyun.com (mirrors.aliyun.com)|183.204.203.226|:80... verbunden.
HTTP-Anforderung gesendet, warte auf Antwort... 200 OK
Länge: 2523 (2,5K) [application/octet-stream]
In »»Centos-7.repo«« speichern.

100%[==================================================================================================>] 2.523       --.-K/s   in 0s      

2022-02-16 16:10:00 (9,07 MB/s) - »»Centos-7.repo«« gespeichert [2523/2523]

[vagrant@tool yum.repos.d]$ ll
insgesamt 4
-rw-r--r-- 1 root root 2523 26. Dez 2020  Centos-7.repo
[vagrant@tool yum.repos.d]$ 
```



### 执行yum源更新命令

命令：yum clean all

命令：yum makecache

注意：依次执行

```
vagrant@tool yum.repos.d]$ sudo yum clean all
Geladene Plugins: fastestmirror
Quellen werden aufgeräumt:base extras updates
Cleaning up everything
[vagrant@tool yum.repos.d]$ sudo yum makecache
Geladene Plugins: fastestmirror
base                                                                                                                 | 3.6 kB  00:00:00     
extras                                                                                                               | 2.9 kB  00:00:00     
updates                                                                                                              | 2.9 kB  00:00:00     
(1/10): base/7/x86_64/group_gz                                                                                       | 153 kB  00:00:00     
base/7/x86_64/primary_db       FAILED                                          
http://mirrors.cloud.aliyuncs.com/centos/7/os/x86_64/repodata/6d0c3a488c282fe537794b5946b01e28c7f44db79097bb06826e1c0c88bad5ef-primary.sqlite.bz2: [Errno 14] curl#6 - "Could not resolve host: mirrors.cloud.aliyuncs.com; Name or service not known"
Anderer Spiegelserver wird versucht.
(2/10): extras/7/x86_64/filelists_db                                                                                 | 259 kB  00:00:00     
(3/10): base/7/x86_64/other_db                                                                                       | 2.6 MB  00:00:00     
(4/10): extras/7/x86_64/other_db                                                                                     | 145 kB  00:00:00     
(5/10): updates/7/x86_64/filelists_db                                                                                | 7.4 MB  00:00:00     
(6/10): updates/7/x86_64/other_db                                                                                    | 960 kB  00:00:00     
(7/10): updates/7/x86_64/primary_db                                                                                  |  13 MB  00:00:00     
(8/10): base/7/x86_64/primary_db                                                                                     | 6.1 MB  00:00:00     
base/7/x86_64/filelists_db     FAILED                                          
http://mirrors.aliyuncs.com/centos/7/os/x86_64/repodata/d6d94c7d406fe7ad4902a97104b39a0d8299451832a97f31d71653ba982c955b-filelists.sqlite.bz2: [Errno 12] Timeout on http://mirrors.aliyuncs.com/centos/7/os/x86_64/repodata/d6d94c7d406fe7ad4902a97104b39a0d8299451832a97f31d71653ba982c955b-filelists.sqlite.bz2: (28, 'Connection timed out after 30214 milliseconds')
Anderer Spiegelserver wird versucht.
(9/10): base/7/x86_64/filelists_db                                                                                   | 7.2 MB  00:00:00     
extras/7/x86_64/primary_db     FAILED                                          
http://mirrors.aliyuncs.com/centos/7/extras/x86_64/repodata/db1c88508275ffebdc6cd8686da08745d2552e5b219b2e6f4cbde7b8afd3b1a3-primary.sqlite.bz2: [Errno 12] Timeout on http://mirrors.aliyuncs.com/centos/7/extras/x86_64/repodata/db1c88508275ffebdc6cd8686da08745d2552e5b219b2e6f4cbde7b8afd3b1a3-primary.sqlite.bz2: (28, 'Connection timed out after 30213 milliseconds')
Anderer Spiegelserver wird versucht.
(10/10): extras/7/x86_64/primary_db                                                                                  | 243 kB  00:00:00     
Determining fastest mirrors
 * base: mirrors.aliyun.com
 * extras: mirrors.aliyun.com
 * updates: mirrors.aliyun.com
Metadaten-Zwischenspeicher erstellt
[vagrant@tool yum.repos.d]$ 
```

### **调整仓库配置**

```
sudo vi /etc/yum.repos.d/CentOS-Base.repo
```

**修改`[base]`和`[updates]`部分**：

- **注释`mirrorlist`行**：在行首添加 `#`
- **启用`baseurl`行**：删除行首的 `#`
- **替换为以下镜像地址**（示例使用中国镜像）：

```
[base]
name=CentOS-$releasever - Base
baseurl=http://mirrors.aliyun.com/centos/$releasever/os/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=os
enabled=1

[updates]
name=CentOS-$releasever - Updates
baseurl=http://mirrors.aliyun.com/centos/$releasever/updates/$basearch/
#mirrorlist=http://mirrorlist.centos.org/?release=$releasever&arch=$basearch&repo=updates
enabled=1
```

### **清理缓存并重试安装**

**清理yum缓存**：

```
sudo yum clean all
sudo rm -rf /var/cache/yum
```

**常用国内镜像源**

- 阿里云：`http://mirrors.aliyun.com/centos/`
- 腾讯云：`http://mirrors.cloud.tencent.com/centos/`
- 清华：`https://mirrors.tuna.tsinghua.edu.cn/centos/`

#### **步骤 1：安装并配置 EPEL 仓库**

EPEL（Extra Packages for Enterprise Linux）是第三方扩展仓库，需手动安装：

1. **手动下载并安装 EPEL RPM 包**（推荐使用国内镜像）：

   ```
   # 使用阿里云镜像
   sudo wget https://mirrors.aliyun.com/epel/epel-release-latest-7.noarch.rpm
   sudo rpm -ivh epel-release-latest-7.noarch.rpm
   ```

   ```
   # 或使用腾讯云镜像
   sudo wget https://mirrors.cloud.tencent.com/epel/epel-release-latest-7.noarch.rpm
   sudo rpm -ivh epel-release-latest-7.noarch.rpm
   ```

#### **步骤 2：修改 EPEL 仓库镜像源**

1. **编辑 EPEL 仓库配置文件**：

   ```
   sudo vi /etc/yum.repos.d/epel.repo
   ```

2. **替换为国内镜像源**（以阿里云为例）：

   ```
   [epel]
   name=Extra Packages for Enterprise Linux 7 - $basearch
   baseurl=https://mirrors.aliyun.com/epel/7/$basearch
   #metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-7&arch=$basearch
   enabled=1
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
   
   [epel-debuginfo]
   name=Extra Packages for Enterprise Linux 7 - $basearch - Debug
   baseurl=https://mirrors.aliyun.com/epel/7/$basearch/debug
   #metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-debug-7&arch=$basearch
   enabled=0
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
   
   [epel-source]
   name=Extra Packages for Enterprise Linux 7 - $basearch - Source
   baseurl=https://mirrors.aliyun.com/epel/7/SRPMS
   #metalink=https://mirrors.fedoraproject.org/metalink?repo=epel-source-7&arch=$basearch
   enabled=0
   gpgcheck=1
   gpgkey=file:///etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-7
   ```

   - **关键操作**：注释 `metalink` 行，启用 `baseurl` 并指向国内镜像。

#### **步骤 3：清理缓存并重试安装**

1. **清除旧缓存**：

   ```
   sudo yum clean all
   sudo rm -rf /var/cache/yum
   ```

2. **重新生成缓存**：

   ```
   sudo yum makecache
   ```

   

### **附：常用国内 EPEL 镜像**

| 镜像源       | `baseurl` 地址                                          |
| :----------- | :------------------------------------------------------ |
| **阿里云**   | `https://mirrors.aliyun.com/epel/7/$basearch`           |
| **腾讯云**   | `https://mirrors.cloud.tencent.com/epel/7/$basearch`    |
| **清华大学** | `https://mirrors.tuna.tsinghua.edu.cn/epel/7/$basearch` |
