### 依赖冲突

依赖冲突是指项目依赖的某一个jar包，有多个不同的版本，因而造成了包版本冲突。

### maven依赖原则

- 路径最近者优先

  相同jar不同版本，根据依赖的路径长短来决定引入哪个依赖。

​        依赖链路一：A -> B -> C -> X(1.0) 

​        依赖链路二：D-> F -> X(2.0)

该例中X(1.0)的路径长度为3,而X(2.0)的路径长度为2,因此X(2.0)会被解析使用。依赖调解第一原则不能解决所有问题，比如这样的依赖关系：依赖路径长度是一样的，都为2.

依赖链路一：A -> B -> C -> X(1.0) 

依赖链路二：D -> E ->  F -> X(2.0)

- 同级(第一级遵循最后、其它级遵循最先)引用原则：

在依赖路径长度相等的前提下，在POM中依赖声明的顺序决定了谁会被解析使用:

在第一级，谁后声明，使用谁

不在第一级，谁先声明，使用谁

#### 传递性依赖

比如当我们项目中，引用了A的依赖,A的依赖通常又会引入B的jar包，B可能还会引入C的jar包。这样，当你在pom.xml文件中添加了A的依赖，Maven会自动的帮你把所有相关的依赖都添加进来。

#### 如何排除依赖

解决依赖冲突的方法，就是使用Maven提供的 `< exclusion >`标签，`< exclusion>`标签需要放在`< exclusions>`标签内部，需要注意的是，声明exclusion的时候只需要`groupld`和`artifactld`,而不需要要version元素。就像下面这样：

```
<dependency>
    <groupId>org.apache.logging.log4j</groupId>
    <artifactId>log4j-core</artifactId>
    <version>2.10.0</version>
    <exclusions>
        <exclusion>
        <artifactId>log4j-api</artifactId>
        <groupId>org.apache.logging.log4j</groupId>
        </exclusion>
    </exclusions>
</dependency>
```

或者使用 dependencyManagement 指定使用哪一个版本的依赖。

dependencyManagement的作用有两个：

- 指定使用某个依赖的哪一个版本

- 统一管理版本

  **在dependencyManagement指定版本号后，在与dependencyManagement同级的dependencies标签里面，引入依赖时，就可以不指定版本号了。**

### 依赖导入失败场景和解决方案

在使用 Maven 构建项目时，可能会发生依赖项下载错误的情况，主要原因有以下几种：

1.  下载依赖时出现网络故障或仓库服务器宕机等原因，导致无法连接至 Maven 仓库，从而无法下载依赖。
2.  依赖项的版本号或配置文件中的版本号错误，或者依赖项没有正确定义，导致 Maven 下载的依赖项与实际需要的不一致，从而引发错误。
3.  本地 Maven 仓库或缓存被污染或损坏，导致 Maven 无法正确地使用现有的依赖项，并且也无法重新下载！

解决方案：

1.  检查网络连接和 Maven 仓库服务器状态。
2.  确保依赖项的版本号与项目对应的版本号匹配，并检查 POM 文件中的依赖项是否正确。
3.  清除本地 Maven 仓库缓存（lastUpdated 文件），因为只要存在lastupdated缓存文件，刷新也不会重新下载。本地仓库中，根据依赖的gav属性依次向下查找文件夹，最终删除内部的文件，刷新重新下载即可！
