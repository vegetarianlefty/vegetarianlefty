### JSR-303 简介

[JSR-303](https://link.jianshu.com/?t=https://jcp.org/en/jsr/detail?id=303) 是 Java EE 6 中的一项子规范，JSR是`Java Specification Requests`的缩写，意思是Java 规范提案，又叫做Bean Validation。JSR 303是Java为bean`数据合法性校验`提供的`标准框架`。Hibernate Validator是Bean Validation的参考实现。

### Hibernate Validator 使用

1. #### 引入依赖

```xml
<!-- springboot下web启动器已经依赖了这个包 -->
<!-- https://mvnrepository.com/artifact/org.hibernate/hibernate-validator -->
<dependency>
    <groupId>org.hibernate</groupId>
    <artifactId>hibernate-validator</artifactId>
    <version>6.1.0.Final</version>
</dependency>
```

2. #### JSR303规范来做参数校验

```java
@Data
public class TestRequest {
  @NotEmpty(message = "姓名不能为空")
  @Length(min = 1, max = 6, message = "姓名长度必须在1-6之间")
  private String name;

  @NotBlank(message = "电话号码不能为空")
  private String phone;

  @Email(message = "邮件格式不正确")
  private String email;

  @NotNull(message = "年龄不能为空")
  @Max(value = 18,message = "年龄不能超过18")
  private Integer age;
}
```

3. #### 用@Valid对controller校验

```java
/**
 * 这里的@Valid也可以换成@Validated效果是一样的
 * @param testBean
 * @return
 */
@GetMapping("test")
public String test(@Valid TestBean testBean) {
    //TODO 自己的逻辑
    return "success";
}
```

##### @Valid与@Validated区别

**@Valid**：javax.validation提供，作用在方法、构造方法、参数、成员属性上，@Valid可做嵌套

**@Validated**：spring提供（在上面的基础上做了扩展），作用在类上、方法、参数，支持分组

### Bean Validation 中内置的 constraint

| Constraint                  |                         详细信息                         |
| :-------------------------- | :------------------------------------------------------: |
| @Null                       |                 被注释的元素必须为 null                  |
| @NotNull                    |                被注释的元素必须不为 null                 |
| @AssertTrue                 |                 被注释的元素必须为 true                  |
| @AssertFalse                |                 被注释的元素必须为 false                 |
| @Min(value)                 | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @Max(value)                 | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @DecimalMin(value)          | 被注释的元素必须是一个数字，其值必须大于等于指定的最小值 |
| @DecimalMax(value)          | 被注释的元素必须是一个数字，其值必须小于等于指定的最大值 |
| @Size(max, min)             |           被注释的元素的大小必须在指定的范围内           |
| @Digits (integer, fraction) |   被注释的元素必须是一个数字，其值必须在可接受的范围内   |
| @Past                       |             被注释的元素必须是一个过去的日期             |
| @Future                     |             被注释的元素必须是一个将来的日期             |
| @Pattern(value)             |           被注释的元素必须符合指定的正则表达式           |

### Hibernate Validator 附加的 constraint

| Constraint |                详细信息                |
| :--------- | :------------------------------------: |
| @Email     |     被注释的元素必须是电子邮箱地址     |
| @Length    | 被注释的字符串的大小必须在指定的范围内 |
| @NotEmpty  |        被注释的字符串的必须非空        |
| @Range     |     被注释的元素必须在合适的范围内     |

