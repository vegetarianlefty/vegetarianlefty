### Maven Archetype 插件介绍

使用 Maven Archetype 创建 Maven 项目

```
# aaa : 项目组(groupId),公司或组织域名的倒序
# bbb : 项目名(artifactId),模块的名称，将来作为 Maven 工程的工程名
# ccc : 项目包名（package），Java项目的包名
# 1.0.0-SNAPSHOT：模块的版本号，SNAPSHOT 表示快照版本，正在迭代过程中，不稳定的版本。RELEASE 表示正式版本
[root@ ~]# mvn archetype:generate -DgroupId=aaa -DartifactId=bbb -Dversion=1.0.0-SNAPSHOT DinteractiveMode=false -Dpackage=ccc
```

