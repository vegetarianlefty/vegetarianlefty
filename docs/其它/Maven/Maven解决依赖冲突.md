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
