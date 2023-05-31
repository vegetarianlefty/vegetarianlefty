### Maven 介绍

什么是Maven呢？我们看下官网给出的一段介绍：

> Apache Maven is a software project management and comprehension tool. Based on the concept of a project object model (POM), Maven can manage a project's build, reporting and documentation from a central piece of information.
>
> 从介绍中我们可以看到Apache Maven是一个项目管理和理解工具，它基于项目对象模型(POM)的概念，它可以管理项目的构建、报告和文档。

maven是一个项目构建和管理的工具，提供了帮助管理 构建、文档、报告、依赖、scms、发布、分发的方法。可以方便的编译代码、进行依赖管理、管理二进制库等等。
maven的好处在于可以将项目过程规范化、自动化、高效化以及强大的可扩展性
利用maven自身及其插件还可以获得代码检查报告、单元测试覆盖率、实现持续集成等等。

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

#### Maven依赖管理

我们能够通过maven坐标确定一个项目，换句话说，我们可以用它来解决依赖关系。在POM中，依赖关系是在dependencies部分中定义的。

**Maven库**

我们所依赖的库是从maven默认的远程库 ([http://repo.maven.org/maven2](https://link.jianshu.com?t=http://repo.maven.org/maven2)) 下载的，这个是公有的库。有时公司自己封装了一些私有库，这个时候我们就可以搭建自己的私有库了。本地库是指maven下载了插件或者jar文件后存放在本地机器上的拷贝。在Mac上，它的位置在~/.m2/repository。当maven查找需要的jar文件时，它会先在本地库中查找，只有在找不到的情况下，才会去远程库中找的。由于maven默认的远程库服务器在国外，国内访问的时候比较慢，建议替换成阿里云的镜像，仓库地址如下：

```ruby
http://maven.aliyun.com/nexus/content/groups/public/
```

