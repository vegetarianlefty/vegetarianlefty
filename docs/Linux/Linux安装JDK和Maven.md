### centos7 安装JDK1.8

查看 JDK 版本

```
[root@tool tool]# java -version 
java version "1.8.0_241" Java(TM) SE Runtime Environment (build 1.8.0_241-b07) Java HotSpot(TM) 64-Bit Server VM (build 25.241-b07, mixed mode)
```

查看 JDK 相关

```
[root@tool tool]#rpm -qa | grep java
```

删除 JDK

```
[root@tool tool]#rpm -e --nodeps java-1.7.0-openjdk-xxxx
```

检查 JDK 版

```
[root@tool tool]# java -version -bash: java: Kommando nicht gefunden.
```

下载 JDK 安装包

https://www.oracle.com/java/technologies/javase/javase-jdk8-downloads.html

注册下载得到文件: jdk-8u241-linux-x64.tar.gz

链接：https://pan.baidu.com/s/1UX3qi6XbJZF8lzlpqFr9Fg
提取码：wajb

把下载 JDK 安装包传输到服务器

![img](http://image.wangxiaohuan.com/blog/image/20210514142656.png)

切换到相关目录解压

```
[root@tool /]# cd usr/java
[root@tool java]# ls
jdk-8u241-linux-x64.tar.gz
[root@tool java]# tar -zxvf jdk-8u241-linux-x64.tar.gz
```

配置 JDK 环境变量，并生效

```
[root@tool /]#vi /etc/profile

#java environment

export JAVA_HOME=/usr/java/jdk1.8.0_241

export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar

export PATH=$PATH:${JAVA_HOME}/bin

[root@tool /]# source /etc/profile
```



### centos7 安装 Maven

下载 Maven 包

```
[root@tool tool]# wget https://mirrors.tuna.tsinghua.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.tar.gz
```

解压 Maven 包

```
[root@tool tool]# tar -xf apache-maven-3.6.3-bin.tar.gz -C /usr/local/
[root@tool tool]# mv /usr/local/apache-maven-3.6.3/ maven3.6
```

配置环境变量

```
[root@tool /]#vi /etc/profile #maven environment export PATH=$PATH:/usr/local/maven3.6/bin
```

验证

**运行maven需要Java环境----系统安装有jdk，并且在系统中配置了JAVA_HOME。**

```
[root@tool tool]# mvn -version Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f) Maven home: /usr/local/maven3.6 Java version: 1.8.0_241, vendor: Oracle Corporation, runtime: /usr/java/jdk1.8.0_241/jre Default locale: de_DE, platform encoding: UTF-8 OS name: "linux", version: "3.10.0-229.el7.x86_64", arch: "amd64", family: "unix"
```

```
vi /etc/profile #java environment export JAVA_HOME=/usr/java/jdk1.8.0_241 export CLASSPATH=.:${JAVA_HOME}/jre/lib/rt.jar:${JAVA_HOME}/lib/dt.jar:${JAVA_HOME}/lib/tools.jar export PATH=$PATH:${JAVA_HOME}/bin
```