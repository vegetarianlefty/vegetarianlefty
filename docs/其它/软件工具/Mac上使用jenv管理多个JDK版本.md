### JDK 介绍

JDK，全称：Java Development Kit，也就是**Java开发人员发布的免费软件开发工具包**。JDK一般包括JRE（Java Runtime Environment），所以我们需要使用Java环境，习惯性叫**配置JDK环境**。

一般，我们需要用到基于Java虚拟机的操作服务时，需要有JDK的环境，这样的Java有两个特点（**Write once, run anywhere**）：

- 易于移植：只需要有Java的虚拟机（JDK）环境，即可运行Java程序。
- 需要平台有Java虚拟机支持：Java程序有所限制，需要有Java环境支持

### JDK选择

在配置JDK前，我们先来看看如何选择JDK。JDK的版本分支很多，大体上分为：

- OpenJDK：开源的JDK
- OracleJDK：Oracle公司开发的JDK

这个其实是看个人喜好，但是如果问我，**我更推荐OpenJDK，避免项目因为版权问题而引发纠纷**（每个程序员，都有个项目做大做强的梦……）

其中，软件包管理器的JDK，也属于OpenJDK的一种。

### OpenJDK

最开始，JDK由SUN公司开发，但是后来被Oracle公司收购，现在的OracleJDK，其License 协议已经从BCL换成了OTN，这就意味着，你不能在生产环境使用这个版本了（可能要被反薅了🥲）。

不过呢？早在2006 年的 Java One 大会上，Sun公司（那时候还没被Oracle收购）就宣布Java技术的开源，随之诞生了OpenJDK，并在随着Google的Android全面使用OpenJDK开始，OpenJDK使用的人数越来越多。

目前，OpenJDK 基本满足所有开发的需求和需要，小部分需要使用 OracleJDK 的包时，基本也可以通过引用外部包和自己编写依赖包解决（如：sun 包下的 Base64）。所以，目前使用 OpenJDK，**不管作为开发还是生产环境，都是不错的选择。**

### JDK 版本选择

JD K的版本怎么选择呢？一般而言，**JDK的版本并不是越高版本越好**。JDK一般是一个稳定长期支持版本，然后经过两个快速开发版本迭代，再出一个稳定长期版本。比如：JDK8 发布，JDK9、JDK10 就是开发版本，之后的JDK11 又是一个稳定长期支持版本。

所以，**通常情况下，我们使用稳定长期支持版本比较好，如：JDK8、JDK11、JDK17.**

***MAC 上安装了多个 JDK 版本，但是根据开发环境的需要如何在这些 JDK 版本之间切换呢？***

#### 这就需要使用 MAC 上的 JAVA 版本管理工具 [jevn](https://www.jenv.be/)

###  Jenv

[Jenv项目地址：https://www.jenv.be](https://www.jenv.be/)
按照官网的解释:
*jEnv is a command line tool to help you forget how to set the JAVA_HOME environment variable*
简单地说：**使用一条命令可以轻松配置JAVA_HOME到环境变量。**

### 安装 Jenv

Homebrew 是一款 Mac OS 平台下的软件包管理工具，拥有安装、卸载、更新、查看、搜索等很多实用的功能。简单的一条指令，就可以实现包管理，而不用你关心各种依赖和文件路径的情况，十分方便快捷。

在[Homebrew](https://link.jianshu.com?t=https%3A%2F%2Fbrew.sh)官网上有详细的安装说明，总结如下：

1. 将以下命令粘贴至终端

```
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

2. 输入以下命令进行安装 jenv
```
    $ brew install jenv
```

3. 安装成功后进行配置
```
$ echo 'export PATH="$HOME/.jenv/bin:$PATH"' >> ~/.bash_profile

$ echo 'eval "$(jenv init -)"' >> ~/.bash_profile

```

4. 输入以下命令查看找到的java版本
```
 $ jenv versions

* system (set by /Users/wangxiaohuan/.jenv/version)

```

5. 下载安装JDK

   [JDK8](https://www.oracle.com/java/technologies/downloads/#java8-mac)，[JDK11](https://www.oracle.com/java/technologies/downloads/#java11-mac)，[JDK17](https://www.oracle.com/java/technologies/downloads/#jdk17-mac) 

   注：*下载下的JDK都存放在 /Library/Java/JavaVirtualMachines/ 目录下*

6. 使用 jenv add 命令将JDK 加入jenv中
```
#添加JDK8
$ jenv add /Library/Java/JavaVirtualMachines/jdk1.8.0_151.jdk/Contents/Home

oracle64-1.8.0.151 added
1.8.0.151 added
1.8 added

#添加JDK11
$ jenv add /Library/Java/JavaVirtualMachines/jdk-11.0.10.jdk/Contents/Home

oracle64-11.0.10 added
11.0.10 added
11.0 added
11 added

#添加JDK17
$ jenv add /Library/Java/JavaVirtualMachines/jdk-17.0.2.jdk/Contents/Home

oracle64-17.0.2 added
17.0.2 added
17.0 added
17 added
```

7. 运行jenv versions 命令
```
$ jenv versions

* system (set by /Users/wangxiaohuan/.jenv/version)
  1.8
  1.8.0.151
  11
  11.0
  11.0.10
  17
  17.0
  17.0.2
  oracle64-1.8.0.151
  oracle64-11.0.10
  oracle64-17.0.2
```

8. 选择一个JDK 版本，运行（ jenv local /jenv global）命令 注 jenv local 当前目录使用 JDK  jenv global 全局 JDK
```
$ jenv local 1.8
jenv local 1.8

#查看JDK 版本
$ java -version
java version "1.8.0_151"
Java(TM) SE Runtime Environment (build 1.8.0_151-b12)
Java HotSpot(TM) 64-Bit Server VM (build 25.151-b12, mixed mode)
```

