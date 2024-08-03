### SpringBoot3+Dubbo3+JDK17

报错提示

```
java.lang.reflect.InaccessibleObjectException: Unable to make field final int java.math.BigInteger.signum accessible: module java.base does not "opens java.math" to unnamed module @75412c2f
```

 

原因是使用了 hessian 进行序列化，hessian 本身不兼容 JDK17

Hessian序列化是一种支持动态类型、跨语言、基于对象传输的网络协议，Java对象序列化的二进制流可以被其他语言（如，c++，python）。特性如下：

1. 自描述序列化类型。不依赖外部描述文件或者接口定义，用一个字节表示常用的基础类型，极大缩短二进制流。
2. 语言无关，支持脚本语言
3. 协议简单，比Java原生序列化高效
4. 相比hessian1，hessian2中增加了压缩编码，其序列化二进制流大小是Java序列化的50%，序列化耗时是Java序列化的30%，反序列化耗时是Java序列化的20%。

Dubbo < 3.2.0 版本中，默认使用 Hessian2 作为默认序列化

解决办法 

使用 Fastjson2 序列化，

```xml
## 添加依赖
<dependencies>
    <dependency>
        <groupId>com.alibaba.fastjson2</groupId>
        <artifactId>fastjson2</artifactId>
        <version>2.0.23</version>
    </dependency>
</dependencies>

#  application.yml (Spring Boot)
dubbo:
 protocol:
   serialization: fastjson2
```

`FASTJSON v2`是`FASTJSON`项目的重要升级，目标是为下一个十年提供一个高性能的`JSON`库。通过同一套`API`，

- 支持`JSON/JSONB`两种协议，[`JSONPath`](https://alibaba.github.io/fastjson2/jsonpath_cn) 是一等公民。
- 支持全量解析和部分解析。
- 支持`Java`服务端、客户端`Android`、大数据场景。
- 支持`Kotlin` https://alibaba.github.io/fastjson2/kotlin_cn
- 支持`JSON Schema` https://alibaba.github.io/fastjson2/json_schema_cn
- 支持`Android 8+`
- 支持`Graal Native-Image`
- 支持 `JSON Schema` https://alibaba.github.io/fastjson2/json_schema_cn

Fastjson2 序列化仅 Dubbo > 3.1.0 版本支持。在 Dubbo > 3.2.0 中将替代 Hessian 作为默认序列化方式。