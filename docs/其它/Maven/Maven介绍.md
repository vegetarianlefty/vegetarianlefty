### Maven 介绍

什么是Maven呢？我们看下官网给出的一段介绍：

> Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.
>
> 从介绍中我们可以看到Apache Maven是一个项目管理和理解工具，它基于项目对象模型(POM)的概念，它可以管理项目的构建、报告和文档。

maven是一个项目构建和管理的工具，提供了帮助管理 构建、文档、报告、依赖、scms、发布、分发的方法。可以方便的编译代码、进行依赖管理、管理二进制库等等。
maven的好处在于可以将项目过程规范化、自动化、高效化以及强大的可扩展性
利用maven自身及其插件还可以获得代码检查报告、单元测试覆盖率、实现持续集成等等。

### Maven工程GAVP属性

> Maven工程相对之前的工程，多出一组gavp属性，gav需要我们在创建项目的时指定，p有默认值，后期通过配置文件修改。既然要填写的属性，我们先行了解下这组属性的含义!

Maven 中的 GAVP 是指 GroupId、ArtifactId、Version、Packaging 等四个属性的缩写，其中前三个是必要的，而 Packaging 属性为可选项。这四个属性主要为每个项目在maven仓库总做一个标识，类似人的《姓-名》。有了具体标识，方便maven软件对项目进行管理和互相引用！

**GAV遵循一下规则：**

 **GroupID 格式**：com.{公司/BU }.业务线.\[子业务线]，最多 4 级。

说明：{公司/BU} 例如：alibaba/taobao/tmall/aliexpress 等 BU 一级；子业务线可选。

正例：com.taobao.tddl 或 com.alibaba.sourcing.multilang  com.atguigu.java

 **ArtifactID 格式**：产品线名-模块名。语义不重复不遗漏，先到仓库中心去查证一下。

正例：tc-client / uic-api / tair-tool / bookstore

**Version版本号格式推荐**：主版本号.次版本号.修订号 1.0.0

主版本号：当做了不兼容的 API 修改，或者增加了能改变产品方向的新功能。

次版本号：当做了向下兼容的功能性新增（新增类、接口等）。

修订号：修复 bug，没有修改方法签名的功能加强，保持 API 兼容性。

例如： 初始→1.0.0  修改bug → 1.0.1  功能调整 → 1.1.1等

**Packaging定义规则：**

指示将项目打包为什么类型的文件，idea根据packaging值，识别maven项目类型！

packaging 属性为 jar（默认值），代表普通的Java工程，打包以后是.jar结尾的文件。

packaging 属性为 war，代表Java的web工程，打包以后.war结尾的文件。

packaging 属性为 pom，代表不会打包，用来做继承的父工程。

### Maven工程项目结构说明

Maven 是一个强大的构建工具，它提供一种标准化的项目结构，可以帮助开发者更容易地管理项目的依赖、构建、测试和发布等任务。以下是 Maven Web 程序的文件结构及每个文件的作用：

```xml
|-- pom.xml                               # Maven 项目管理文件 
|-- src
    |-- main                              # 项目主要代码
    |   |-- java                          # Java 源代码目录
    |   |   `-- com/example/myapp         # 开发者代码主目录
    |   |       |-- controller            # 存放 Controller 层代码的目录
    |   |       |-- service               # 存放 Service 层代码的目录
    |   |       |-- dao                   # 存放 DAO 层代码的目录
    |   |       `-- model                 # 存放数据模型的目录
    |   |-- resources                     # 资源目录，存放配置文件、静态资源等
    |   |   |-- log4j.properties          # 日志配置文件
    |   |   |-- spring-mybatis.xml        # Spring Mybatis 配置文件
    |   |   `-- static                    # 存放静态资源的目录
    |   |       |-- css                   # 存放 CSS 文件的目录
    |   |       |-- js                    # 存放 JavaScript 文件的目录
    |   |       `-- images                # 存放图片资源的目录
    |   `-- webapp                        # 存放 WEB 相关配置和资源
    |       |-- WEB-INF                   # 存放 WEB 应用配置文件
    |       |   |-- web.xml               # Web 应用的部署描述文件
    |       |   `-- classes               # 存放编译后的 class 文件
    |       `-- index.html                # Web 应用入口页面
    `-- test                              # 项目测试代码
        |-- java                          # 单元测试目录
        `-- resources                     # 测试资源目录
```

- pom.xml：Maven 项目管理文件，用于描述项目的依赖和构建配置等信息。
- src/main/java：存放项目的 Java 源代码。
- src/main/resources：存放项目的资源文件，如配置文件、静态资源等。
- src/main/webapp/WEB-INF：存放 Web 应用的配置文件。
- src/main/webapp/index.html：Web 应用的入口页面。
- src/test/java：存放项目的测试代码。
- src/test/resources：存放测试相关的资源文件，如测试配置文件等。

### 约定优于配置

Maven使用约定优于配置的原则，如下所示：

| 目录                               | 目的                                                         |
| ---------------------------------- | ------------------------------------------------------------ |
| ${basedir}                         | 存放pom.xml和所有的子目录                                    |
| ${basedir}/src/main/java           | 项目的java源代码                                             |
| ${basedir}/src/main/resources      | 项目的资源，比如说property文件，springmvc.xml                |
| ${basedir}/src/test/java           | 项目的测试类，比如说Junit代码                                |
| ${basedir}/src/test/resources      | 测试用用的资源                                               |
| ${basedir}/src/main/webapp/WEB-INF | web应用文件目录，web项目的信息，比如存放web.xml、本地图片、jsp视图页面 |
| ${basedir}/target                  | 打包输出目录                                                 |
| ${basedir}/target/classes          | 编译输出目录                                                 |
| ${basedir}/target/test-classes     | 测试编译输出目录                                             |
| Test.java                          | Maven只会自动运行符合该命名规则的测试类                      |
| ~/.m2/repository                   | Maven默认的本地仓库目录位置                                  |

一个maven项目在默认情况下会产生jar文件，另外，编译后的classes会放在${basedir}/target/classes下面，jar文件会放在${basedir}/target下面。使用约定优于配置带来最大的好处就是项目的统一，任何人在使用Maven项目的时候，文件的存放位置都是一样的，通用性比较好。这也是为什么我们在用intellij创建一个maven项目的时候，需要配置源文件、资源文件路径的原因。

### maven的核心概念介绍

#### POM(Project Object Model)

pom是指project object Model。pom是一个xml，在maven2里为pom.xml。是maven工作的基础，在执行task或者goal时，maven会去项目根目录下读取pom.xml获得需要的配置信息
pom文件中包含了项目的信息和maven build项目所需的配置信息，通常有项目信息(如版本、成员)、项目的依赖、插件和goal、build选项等等
pom是可以继承的，通常对于一个大型的项目或是多个module的情况，子模块的pom需要指定父模块的pom

pom文件中节点含义如下：

```
project pom文件的顶级元素
modelVersion 所使用的object model版本，为了确保稳定的使用，这个元素是强制性的。除非maven开发者升级模板，否则不需要修改
groupId 是项目创建团体或组织的唯一标志符，通常是域名倒写，如groupId  org.apache.maven.plugins就是为所有maven插件预留的
artifactId 是项目artifact唯一的基地址名
packaging artifact打包的方式，如jar、war、ear等等。默认为jar。这个不仅表示项目最终产生何种后缀的文件，也表示build过程使用什么样的lifecycle。
version artifact的版本，通常能看见为类似0.0.1-SNAPSHOT，其中SNAPSHOT表示项目开发中，为开发版本
name 表示项目的展现名，在maven生成的文档中使用
url表示项目的地址，在maven生成的文档中使用
description 表示项目的描述，在maven生成的文档中使用
dependencies 表示依赖，在子节点dependencies中添加具体依赖的groupId artifactId和version
build 表示build配置
parent 表示父pom
```

#### Maven插件

Maven常用的插件比如compiler插件、surefire插件、jar插件。比如说jar插件包含建立jar文件的目标，compiler插件包含编译源代码和单元测试代码的目标，surefire插件则是运行单元测试的目标。为什么需要这些插件呢？因为maven本身不会做太多的事情，它不知道怎么样编译或者怎么样打包。它把构建的任务交给插件去做。**插件定义了常用的构建逻辑，能够被重复利用。这样做的好处是，一旦插件有了更新，那么所有maven用户都能得到更新。**

#### Maven生命周期

生命周期指项目的构建过程，它包含了一系列的有序的阶段，而一个阶段就是构建过程中的一个步骤，比如package阶段、compiler阶段等。那么生命周期阶段和上面说的插件目标之间是什么关系呢？**插件目标可以绑定到生命周期阶段上，一个生命周期可以绑定多个插件目标。当maven在构建过程中逐步的通过每个阶段时，会执行该阶段所有的插件目标。**目前生命周期阶段有clean、vavidate、compiler、test、package、verify、install、site、deploy阶段。

![image_9Dq3EsALqa](http://image.wangxiaohuan.com/blog/image/202308260933451.png)

#### Maven依赖管理

Maven 可以管理项目的依赖，包括自动下载所需依赖库、自动下载依赖需要的依赖并且保证版本没有冲突、依赖版本管理等。通过 Maven，我们可以方便地维护项目所依赖的外部库，而我们仅仅需要编写配置即可。我们能够通过maven坐标确定一个项目，换句话说，我们可以用它来解决依赖关系。在POM中，依赖关系是在dependencies部分中定义的。

### **Maven库**

我们所依赖的库是从maven默认的远程库 ([http://repo.maven.org/maven2](https://link.jianshu.com?t=http://repo.maven.org/maven2)) 下载的，这个是公有的库。有时公司自己封装了一些私有库，这个时候我们就可以搭建自己的私有库了。本地库是指maven下载了插件或者jar文件后存放在本地机器上的拷贝。在Mac上，它的位置在~/.m2/repository。当maven查找需要的jar文件时，它会先在本地库中查找，只有在找不到的情况下，才会去远程库中找的。由于maven默认的远程库服务器在国外，国内访问的时候比较慢，建议替换成阿里云的镜像，仓库地址如下：

```ruby
http://maven.aliyun.com/nexus/content/groups/public/
```

### 依赖管理和配置

Maven 依赖管理是 Maven 软件中最重要的功能之一。Maven 的依赖管理能够帮助开发人员自动解决软件包依赖问题，使得开发人员能够轻松地将其他开发人员开发的模块或第三方框架集成到自己的应用程序或模块中，避免出现版本冲突和依赖缺失等问题。

我们通过定义 POM 文件，Maven 能够自动解析项目的依赖关系，并通过 Maven **仓库自动**下载和管理依赖，从而避免了手动下载和管理依赖的繁琐工作和可能引发的版本冲突问题。

### 依赖传递和冲突

**依赖传递**指的是当一个模块或库 A 依赖于另一个模块或库 B，而 B 又依赖于模块或库 C，那么 A 会间接依赖于 C。这种依赖传递结构可以形成一个依赖树。当我们引入一个库或框架时，构建工具（如 Maven、Gradle）会自动解析和加载其所有的直接和间接依赖，确保这些依赖都可用。

依赖传递的作用是：

1.  减少重复依赖：当多个项目依赖同一个库时，Maven 可以自动下载并且只下载一次该库。这样可以减少项目的构建时间和磁盘空间。
2.  自动管理依赖: Maven 可以自动管理依赖项，使用依赖传递，简化了依赖项的管理，使项目构建更加可靠和一致。
3.  确保依赖版本正确性：通过依赖传递的依赖，之间都不会存在版本兼容性问题，确实依赖的版本正确性！

依赖冲突演示：

当直接引用或者间接引用出现了相同的jar包! 这时呢，一个项目就会出现相同的重复jar包，这就算作冲突！依赖冲突避免出现重复依赖，并且终止依赖传递！

![](http://image.wangxiaohuan.com/blog/image/202308260943101.png)

maven自动解决依赖冲突问题能力，会按照自己的原则，进行重复依赖选择。同时也提供了手动解决的冲突的方式，不过不推荐！

解决依赖冲突（如何选择重复依赖）方式：

1. 自动选择原则

   - 短路优先原则（第一原则）

     A—>B—>C—>D—>E—>X(version 0.0.1)

     A—>F—>X(version 0.0.2)

     则A依赖于X(version 0.0.2)。

   - 依赖路径长度相同情况下，则“先声明优先”（第二原则）

     A—>E—>X(version 0.0.1)

     A—>F—>X(version 0.0.2)

     在\<depencies>\</depencies>中，先声明的，路径相同，会优先选择！

### 依赖导入失败场景和解决方案

在使用 Maven 构建项目时，可能会发生依赖项下载错误的情况，主要原因有以下几种：

1.  下载依赖时出现网络故障或仓库服务器宕机等原因，导致无法连接至 Maven 仓库，从而无法下载依赖。
2.  依赖项的版本号或配置文件中的版本号错误，或者依赖项没有正确定义，导致 Maven 下载的依赖项与实际需要的不一致，从而引发错误。
3.  本地 Maven 仓库或缓存被污染或损坏，导致 Maven 无法正确地使用现有的依赖项，并且也无法重新下载！

解决方案：

1.  检查网络连接和 Maven 仓库服务器状态。
2.  确保依赖项的版本号与项目对应的版本号匹配，并检查 POM 文件中的依赖项是否正确。
3.  清除本地 Maven 仓库缓存（lastUpdated 文件），因为只要存在lastupdated缓存文件，刷新也不会重新下载。本地仓库中，根据依赖的gav属性依次向下查找文件夹，最终删除内部的文件，刷新重新下载即可！

### 扩展构建管理和插件配置

**构建概念:**

项目构建是指将源代码、依赖库和资源文件等转换成可执行或可部署的应用程序的过程，在这个过程中包括编译源代码、链接依赖库、打包和部署等多个步骤。

![](http://image.wangxiaohuan.com/blog/image/202308260942791.png)

**主动触发场景：**

-   重新编译 : 编译不充分, 部分文件没有被编译!
-   打包 : 独立部署到外部服务器软件,打包部署
-   部署本地或者私服仓库 : maven工程加入到本地或者私服仓库,供其他工程使用

**命令方式构建:**

语法: mvn 构建命令  构建命令....

| 命令        | 描述                                        |
| ----------- | ------------------------------------------- |
| mvn clean   | 清理编译或打包后的项目结构,删除target文件夹 |
| mvn compile | 编译项目，生成target文件                    |
| mvn test    | 执行测试源码 (测试)                         |
| mvn site    | 生成一个项目依赖信息的展示页面              |
| mvn package | 打包项目，生成war / jar 文件                |
| mvn install | 打包后上传到maven本地仓库(本地部署)         |
| mvn deploy  | 只打包，上传到maven私服仓库(私服部署)       |

**可视化方式构建:**

![](http://image.wangxiaohuan.com/blog/image/202308260943308.png)

**构建命令周期:**

构建生命周期可以理解成是一组固定构建命令的有序集合，触发周期后的命令，会自动触发周期前的命令！也是一种简化构建的思路!

- 清理周期：主要是对项目编译生成文件进行清理

  包含命令：clean

- 默认周期：定义了真正构件时所需要执行的所有步骤，它是生命周期中最核心的部分

  包含命令：compile - test - package - install / deploy

- 报告周期

  包含命令：site

  打包: mvn clean package 本地仓库: mvn clean install

**最佳使用方案:**

```纯文本
打包: mvn clean package
重新编译: mvn clean compile
本地部署: mvn clean install 
```

**周期，命令和插件:**

周期→包含若干命令→包含若干插件!

使用周期命令构建，简化构建过程！

最终进行构建的是插件！

### Maven工程继承关系

1. 继承概念

   Maven 继承是指在 Maven 的项目中，让一个项目从另一个项目中继承配置信息的机制。继承可以让我们在多个项目中共享同一配置信息，简化项目的管理和维护工作。

   ![](http://image.wangxiaohuan.com/blog/image/202308260943646.png)

2. 继承作用

   作用：在父工程中统一管理项目中的依赖信息,进行统一版本管理!

   它的背景是：

   -   对一个比较大型的项目进行了模块拆分。
   -   一个 project 下面，创建了很多个 module。
   -   每一个 module 都需要配置自己的依赖信息。
       它背后的需求是：
   -   多个模块要使用同一个框架，它们应该是同一个版本，所以整个项目中使用的框架版本需要统一管理。
   -   使用框架时所需要的 jar 包组合（或者说依赖信息组合）需要经过长期摸索和反复调试，最终确定一个可用组合。这个耗费很大精力总结出来的方案不应该在新的项目中重新摸索。
       通过在父工程中为整个项目维护依赖信息的组合既保证了整个项目使用规范、准确的 jar 包；又能够将以往的经验沉淀下来，节约时间和精力。

3. 继承语法

   -   父工程

   ```xml
   <groupId>com.atguigu.maven</groupId>
   <artifactId>pro03-maven-parent</artifactId>
   <version>1.0-SNAPSHOT</version>
   <!-- 当前工程作为父工程，它要去管理子工程，所以打包方式必须是 pom -->
   <packaging>pom</packaging>
   
   ```

   -   子工程

   ```xml
   <!-- 使用parent标签指定当前工程的父工程 -->
   <parent>
     <!-- 父工程的坐标 -->
     <groupId>com.atguigu.maven</groupId>
     <artifactId>pro03-maven-parent</artifactId>
     <version>1.0-SNAPSHOT</version>
   </parent>
   
   <!-- 子工程的坐标 -->
   <!-- 如果子工程坐标中的groupId和version与父工程一致，那么可以省略 -->
   <!-- <groupId>com.atguigu.maven</groupId> -->
   <artifactId>pro04-maven-module</artifactId>
   <!-- <version>1.0-SNAPSHOT</version> -->
   ```

4. 父工程依赖统一管理

   - 父工程声明版本

     ```xml
     <!-- 使用dependencyManagement标签配置对依赖的管理 -->
     <!-- 被管理的依赖并没有真正被引入到工程 -->
     <dependencyManagement>
       <dependencies>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-core</artifactId>
           <version>4.0.0.RELEASE</version>
         </dependency>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-beans</artifactId>
           <version>4.0.0.RELEASE</version>
         </dependency>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-context</artifactId>
           <version>4.0.0.RELEASE</version>
         </dependency>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-expression</artifactId>
           <version>4.0.0.RELEASE</version>
         </dependency>
         <dependency>
           <groupId>org.springframework</groupId>
           <artifactId>spring-aop</artifactId>
           <version>4.0.0.RELEASE</version>
         </dependency>
       </dependencies>
     </dependencyManagement>
     ```

   - 子工程引用版本

     ```xml
     <!-- 子工程引用父工程中的依赖信息时，可以把版本号去掉。  -->
     <!-- 把版本号去掉就表示子工程中这个依赖的版本由父工程决定。 -->
     <!-- 具体来说是由父工程的dependencyManagement来决定。 -->
     <dependencies>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-core</artifactId>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-beans</artifactId>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-context</artifactId>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-expression</artifactId>
       </dependency>
       <dependency>
         <groupId>org.springframework</groupId>
         <artifactId>spring-aop</artifactId>
       </dependency>
     </dependencies>
     ```

### 4.2 Maven工程聚合关系

1. 聚合概念

   Maven 聚合是指将多个项目组织到一个父级项目中，通过触发父工程的构建,统一按顺序触发子工程构建的过程!!

2. 聚合作用

   1.  统一管理子项目构建：通过聚合，可以将多个子项目组织在一起，方便管理和维护。
   2.  优化构建顺序：通过聚合，可以对多个项目进行顺序控制，避免出现构建依赖混乱导致构建失败的情况。

3. 聚合语法

   父项目中包含的子项目列表。

   ```xml
   <project>
     <groupId>com.example</groupId>
     <artifactId>parent-project</artifactId>
     <packaging>pom</packaging>
     <version>1.0.0</version>
     <modules>
       <module>child-project1</module>
       <module>child-project2</module>
     </modules>
   </project>
   ```

   
