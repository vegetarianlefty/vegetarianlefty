### 自定义注解

```
注解的声明：
public @interface 注解名称
 * 元注解：标记注解的注解
 * @Documented：表示该注解会被javadoc命令写入api文档中
 * @Target：注解的标记位置
 *  ElementType.ANNOTATION_TYPE：该注解可以标记别的注解
 *  ElementType.CONSTRUCTOR：标注到构造方法
 *  ElementType.FIELD：标注到成员属性
 *  ElementType.LOCAL_VARIABLE：标注到局部变量
 *  ElementType.METHOD：标注到方法上
 *  ElementType.PACKAGE：标注到包上
 *  ElementType.PARAMETER：标注到方法形参上
 *  ElementType.TYPE：标注到类、接口、枚举类上
 * @Retention：注解的作用范围
 *  RetentionPolicy.SOURCE：注解的有效范围只在源码中，编译后就被丢弃
 *  RetentionPolicy.CLASS：注解有效范围在编译文件中，运行时丢弃
 *  RetentionPolicy.RUNTIME：注解在运行时仍然有效，这个范围的注解可以通过反射获取
 
  * 注解内的方法声明：
 * 类型 方法名() [defualt 默认值];
 * 注意：
 * 如果一个属性没有设置default默认值，在标记这个注解时，必须给定该属性值
 * 如果一个属性的名字为value,则在赋值时可以省略属性名。当如果需要赋值两个以上的属性，则value不能省略
 * 如果一个属性的类型是数组类型，则应该用{}赋值，如果只要给一个值，{}可以省略


@Target(ElementType.METHOD,ElementType.TYPE)//作用范围,METHOD 方法上  TYPE 类、接口、枚举类上
@Retention(RetentionPolicy.RUNTIME)//运行时
public @interface Login {

    boolean value() default false;//是否强制需要登录

}

```



### AOP 切面 Aspect

```
@Aspect
@Component
public class LoginAspect {
    /**
     *作用在类上生效
     */
    @Around("@within(login)")
    public Object clazz(ProceedingJoinPoint pjp, Login login) throws Throwable {
        return checkLogin(pjp, login);
    }

    /**
     *作用在方法上生效
     */
    @Around("@annotation(login)")
    public Object menthod(ProceedingJoinPoint pjp, Login login) throws Throwable {
        return checkLogin(pjp, login);
    }
    
    /**
     * 登录逻辑校验
     *
     * @param pjp   切点
     * @return
     * @throws Throwable
     */
    public Object checkLogin(ProceedingJoinPoint pjp,Login login) throws Throwable {
        AuthContext.clearLoginInfo();
        HttpServletRequest request = ((ServletRequestAttributes) RequestContextHolder.getRequestAttributes()).getRequest();
        String userData = URLDecoder.decode(request.getHeader("user-data"),StandardCharsets.UTF_8.toString());
        log.info("登录用户信息:{}", userData);
        AuthUserInfo authUserInfo= JSONUtil.toBean(userData, AuthUserInfo.class);
        AuthContext.setLoginInfo(authUserInfo);
        return pjp.proceed();
    }
}
```

