### IDEA 插件介绍

![图片](http://image.wangxiaohuan.com/blog/image/202210171125643.png)

## 1. lombok

之前对lombok还有争议，到底该不该在项目中使用，现在新版的idea已经内置了lombok插件，所以用它是一种趋势。

我之所以把lombok放在整篇文章的第一个介绍，是因为它真的可以帮我少写很多代码，特别是entity、DTO、VO、BO中的。

我们用User类举例，以前定义javabean需要写如下代码：

```
public class User {

    private Long id;
    private String name;
    private Integer age;
    private String address;

    public User() {

    }

    public User(Long id, String name, Integer age, String address) {
        this.id = id;
        this.name = name;
        this.age = age;
        this.address = address;
    }

    public Long getId() {
        return id;
    }

    public String getName() {
        return name;
    }

    public Integer getAge() {
        return age;
    }

    public String getAddress() {
        return address;
    }


    public void setId(Long id) {
        this.id = id;
    }

    public void setName(String name) {
        this.name = name;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public boolean equals(Object o) {
        if (this == o) returntrue;
        if (o == null || getClass() != o.getClass()) returnfalse;
        User user = (User) o;
        return Objects.equals(id, user.id) &&
                Objects.equals(name, user.name) &&
                Objects.equals(age, user.age) &&
                Objects.equals(address, user.address);
    }

    @Override
    public int hashCode() {
        return Objects.hash(id, name, age, address);
    }

    @Override
    public String toString() {
        return"User{" +
                "id=" + id +
                ", name='" + name + '\'' +
                ", age=" + age +
                ", address='" + address + '\'' +
                '}';
    }
}
```

该User类中包含了：成员变量、getter/setter方法、构造方法、equals、hashCode方法。

咋一看，代码还是挺多的。而且还有个问题，如果User类中的代码修改了，比如：age字段改成字符串类型，或者name字段名称修改了，是不是需要同步修改相关的成员变量、getter/setter方法、构造方法、equals、hashCode方法全都修改一遍？

好消息是用lombok可以解决这个问题。

如果是idea2020.3之前的版本，需要在idea中安装如下插件：![图片](http://image.wangxiaohuan.com/blog/image/202210171126002.png)但idea2020.3之后，idea已经内置了lombok的功能。

有了lombok插件，现在我们在idea只用这样写代码，就能实现上面的功能了：

```
@ToString
@EqualsAndHashCode
@NoArgsConstructor
@AllArgsConstructor
@Getter
@Setter
public class User {

    private Long id;
    private String name;
    private Integer age;
    private String address;
}
```

简直太轻松了，真的可以少写很多代码。

> 此外，我们还需要在项目的pom文件中，引入lombok的依赖包，不然项目会跑不起来。

## 2. Free Mybatis plugin

在国内`mybatis`已经成为了最主流的数据库框架了，该框架属于半自动化的ORM持久化框架，相对于hibernate这种全自动化的持久化框架更灵活，性能更高。

在`mybatis`中，我们需要自己定义mapper和对应的xml文件完成绑定。

在这里我们以用户表为例，首先需要定义UserMapper接口：

```
public interface UserMapper {
  int insertUser(UserModel user);
}
```

然后需要UserMapper.xml配置文件：

```
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.sue.jump.mappers.UserMapper">

    <sql id="selectUserVo">
        id, name, age, sex
     </sql>

    <insert id="insertUser" parameterType="com.sue.jump.model.UserModel">
        INSERT INTO user
        <trim prefix="(" suffix=")" suffixOverrides=",">
            <if test="id != null ">
                id,
            </if>
            <if test="name != null  and name != ''">
                name,
            </if>
            <if test="age != null ">
                age,
            </if>
            <if test="sex != null ">
                sex,
            </if>
        </trim>
        <trim prefix="values (" suffix=")" suffixOverrides=",">
            <if test="id != null ">
                #{id},
            </if>
            <if test="name != null  and name != ''">
                #{name},
            </if>
            <if test="age != null ">
                #{age},
            </if>
            <if test="sex != null ">
                #{sex},
            </if>
        </trim>
    </insert>
</mapper>
```

UserMapper.xml文件中，mapper标签的namespace对应UserMapper接口名，而insert标签的id=insertUser，正好对应UserMapper接口中的insertUser方法。

那么，在项目中如何通过UserMapper类中的getUser方法，能够快速访问UserMapper.xml文件中的getUser方法？

答：这就需要使用`Free Mybatis plugin`插件了。

![图片](http://image.wangxiaohuan.com/blog/image/202210171126205.png)

安装了该插件之后，在UserMapper接口的接口名和方法名的左边，会多了两个绿色的箭头，我们点击该箭头，就能跳转到UserMapper.xml文件对应的mapper标签或者insertUser语句上。![图片](http://image.wangxiaohuan.com/blog/image/202210171126056.png)此外，在UserMapper.xml文件的insertUser语句的左边，也会多出一个绿色的箭头，我们点击该箭头，也能跳转到UserMapper接口的insertUser方法上。![图片](http://image.wangxiaohuan.com/blog/image/202210171127229.png)有了这个插件，我们就能在mapper和xml之间自由切换，自由玩耍了，再也不用像以前那样搜索来搜索去。

## 3.Translation

有些小伙伴，包括我自己可能英语不太好（我英语刚过四级）。

我们在给变量或者方法取名时，要想半天。特别是在阅读JDK英文文档时，遇到了一些生僻字，简直头大。

有个好消息是使用：`Translation`插件，能够让我们在文档中自由飞翔。![图片](http://image.wangxiaohuan.com/blog/image/202210171127198.png)安装完`Translation`插件之后，在other settings中多了一个Translation菜单。

点击该菜单：![图片](http://image.wangxiaohuan.com/blog/image/202210171127324.png)在右边的窗口中，可以选择翻译软件。

选中需要翻译的英文文档：![图片](http://image.wangxiaohuan.com/blog/image/202210171127930.png)在右键弹窗的窗口中，选择Translation选项，会弹如下窗口：![图片](http://image.wangxiaohuan.com/blog/image/202210171127290.png)一段英文段落，一下子翻译成了中文，简直太爽了。

## 4.Alibaba Java Coding Guidelines

如果你是从事Java开发工作的小伙伴，肯定看过阿里巴巴的《Java开发手册》。

该手册总结了我们在日常开发过程中，可能会遇到的问题。从编程规约、异常日志、单位测试、安全规约、Mysql数据库和工程结构，这6大方面，规范了开发的流程，确保我们能写出高效、优雅的代码。

但这些规范性的东西，仅仅靠人的自觉性，很难达到预期的效果。

为了解决这个问题，阿里巴巴推出了`Alibaba Java Coding Guidelines`插件，能够通过该插件，直接查出不合规范的代码。

![图片](http://image.wangxiaohuan.com/blog/image/202210171127284.png)

安装了该插件之后，按下快捷键：`Ctrl+Alt+Shift+J`，可以可对整个项目或单个文件进行编码规约扫描。

![图片](http://image.wangxiaohuan.com/blog/image/202210171127922.png)扫描后会将不规范的代码按从高到低。

目前有三个等级显示在下方：

- Blocker 崩溃
- Critical 严重
- Major 重要

![图片](http://image.wangxiaohuan.com/blog/image/202210171127278.png)点击左边其中一个不规范的代码行，右边窗口会立刻显示不规范的详细代码，便于我们快速定位问题。

nice。

## 5. GenerateAllSetter

很多时候，我们需要给某个对象赋值，如果参数比较多的话，需要手写大量的`setter`或者`getter`代码。

有没有办法一键搞定呢？

答：有，使用`GenerateAllSetter`插件。

![图片](http://image.wangxiaohuan.com/blog/image/202210171127214.png)

安装完插件之后，在创建的对象上，按快捷键下：`alt + enter`。

在弹出的窗口中选择：Generate all setter with default value。![图片](http://image.wangxiaohuan.com/blog/image/202210171127130.png)就会自动生成如下代码：![图片](http://image.wangxiaohuan.com/blog/image/202210171127588.png)简直太方便了。

## 6. SequenceDiagram

我们平时在阅读源码时，为了梳理清楚内部逻辑，经常需要画一些`时序图`。

如果我们直接画，会浪费很多时间，而且画的图不一定正确。

这时可以使用：`SequenceDiagram`插件。

![图片](http://image.wangxiaohuan.com/blog/image/202210171127423.png)选择具体某个方法，右键选择：sequence diagram选项：![图片](http://image.wangxiaohuan.com/blog/image/202210171128493.png)之后，会出现时序图：![图片](http://image.wangxiaohuan.com/blog/image/202210171128084.png)

从此以后，能够成为画图高手了，完美。

## 7. CheckStyle-IDEA

在代码格式方面，有许多地方，需要我们注意，比如：无用导入、没写注释、语法错误、方法太长等等。

有没有办法，可以在idea中，一次性检测出上面的这些问题呢？

答：使用`CheckStyle-IDEA`插件。

`CheckStyle-IDEA`是一个检测代码格式是否满足规范的工具，其中用得比较多的是`Google`规范和`Sun`规范。

![图片](http://image.wangxiaohuan.com/blog/image/202210171128711.png)安装完插件后，在idea的下方会出现：CheckStyle选项：![图片](http://image.wangxiaohuan.com/blog/image/202210171128732.png)点击左边的绿色按钮，可以扫描代码。在中间位置，会显示不符合代码规范的原因。

双击代码，即可直接跳转到具体代码：![图片](http://image.wangxiaohuan.com/blog/image/202210171128380.png)

## 8.JRebel and XRebel

在idea中开发Java项目，有个很不爽的地方是：每次修改一个类或者接口，都需要重启服务，否则不会运行最新地方。

而每次重启，都需要花大量的时间。

有没有办法，Java代码修改后不用重启系统，立即生效呢？

答：使用`JRebel and XRebel`插件。

如图：![图片](http://image.wangxiaohuan.com/blog/image/202210171128136.png)

安装完成之后，这里会有两个绿色的按钮，并且在右边多了一个选项Select Rebel Agents：![图片](http://image.wangxiaohuan.com/blog/image/202210171128401.png)其中一个绿色的按钮，表示热部署启动项目，另外一个表示用debug默认热部署启动项目。

Select Rebel Agents选项中包含三个值：

- JRebel：修改完代码，不重启服务，期望代码直接生效。
- XRebel：请求过程中，各个部分代码性能监控。例如：方法执行时间，出现的异常，SQL执行时间，输出的Log，MQ执行时间等。
- JRebel+XRebel：修改完代码，不重启服务，并且监控代码。

## 9. Codota

说实话，idea现有的代码提示功能，已经很强大了。

但如果你使用过`Codota`插件，它会让你写代码的速度更上一层楼。![图片](http://image.wangxiaohuan.com/blog/image/202210171128771.png)安装完插件之后，我们在写代码时，它会给你一些提示：![图片](http://image.wangxiaohuan.com/blog/image/202210171128089.png)这些提示是基于ai统计出来的，非常有参考价值。

## 10. GsonFormat

很多时候，我需要把`json`中的参数，转换成`实体对象`中的参数。或者把`实体对象`中的参数，转换成`json`中的参数。

以前我们都是手动一个变量，一个变量的拷贝的。

但现在有个好消息是，idea的`GsonFormat`插件可以帮我们完成这件事。

![图片](http://image.wangxiaohuan.com/blog/image/202210171128890.png)安装完插件之后，先创建一个空类：![图片](https://mmbiz.qpic.cn/mmbiz_png/ibJZVicC7nz5ghsXctaSfl1fzZbxibbDXb5SgmaVqmvZvsFJrz5uN91GIqGmeSkhiaia1V63ngRC8qViaVpyy8q0t5eA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)按下快捷键：`alt + s`，会弹出下面这个窗口：![图片](http://image.wangxiaohuan.com/blog/image/202210171129707.png)然后在该窗口中，录入json数据。

点击确定按钮，就会自动生成这些代码：![图片](http://image.wangxiaohuan.com/blog/image/202210171129743.png)简直帅呆了。

## 11. Rainbow Brackets

我们平时写代码的时候，括号是让我们非常头疼的地方，特别是代码逻辑很多，层层嵌套的情况。

一眼很难看出，代码是从哪个括号开始，到哪个反括号结束的。

有没有办法解决这个问题呢？

答：使用`Rainbow Brackets`插件。

![图片](http://image.wangxiaohuan.com/blog/image/202210171129484.png)安装完插件之后，括号和反括号，在代码中会自动按照不同颜色做区分：![图片](http://image.wangxiaohuan.com/blog/image/202210171129165.png)非常显目，非常直观。

## 12. CodeGlance

有些时候，我们阅读的代码很多，比如某个类中包含的方法和成员变量很多。

从上往下，一点点往下翻，会浪费很多时间。那么有没有办法，能够快速翻到想看的代码呢？

答：有，可以使用`CodeGlance`插件。![图片](http://image.wangxiaohuan.com/blog/image/202210171129735.png)安装完插件之后，在代码右侧，会出现下面这个窗口：![图片](http://image.wangxiaohuan.com/blog/image/202210171129234.png)它是代码的缩略图，通过它我们能够非常快速的切换代码块。



### google-java-format —— 代码自动格式化

这个插件的优点在于不需要手动快捷键去格式化代码