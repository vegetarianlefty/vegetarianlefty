### 命令行方式跳过测试

通过使用命令将项目打包，添加跳过测试的命令就可以了，可以用两种命令来跳过测试：

```

mvn package -DskipTests=true

mvn package -Dmaven.test.skip=true
```

在使用 mvn package 进行编译、打包时，Maven会执行 `src/test/java` 中的 JUnit 测试用例，有时为了跳过测试，会使用参数 `-DskipTests=true` 和 `-Dmaven.test.skip=true`，这两个参数的主要区别是：

- 使用 `-Dmaven.test.skip=true`，不但跳过单元测试的运行，也跳过测试代码的编译；
- 使用 `-DskipTests=true` 跳过单元测试，但是会继续编译。

### pom.xml中配置跳过测试

可以在 pom.xml 中添加如下配置来跳过测试：

```
<build>
    <plugins>
        <!-- maven 打包时跳过测试 -->
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-surefire-plugin</artifactId>
            <configuration>
                <skip>true</skip>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### IDEA直接配置

Maven命令栏的工具栏有下图中的图标，这个图标就是 `Skip Tests`。点击选中，再用 `LifeStyle` 中的打包就会跳过测试。

![image-20230620095129648](http://image.wangxiaohuan.com/blog/image/202306200951951.png)

### 添加Maven配置参数

打开IDEA 配置项 Build,Exxcution,Deployment –> Maven Tools –> Maven –> Runner，配置 VM Options  

`-DskipTests=true`  / `-Dmaven.test.skip=true`   或者 勾选上 Skip Tests 选项

![image-20230620095613258](http://image.wangxiaohuan.com/blog/image/202306200956492.png)