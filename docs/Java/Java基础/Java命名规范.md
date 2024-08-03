### 为什么要 Java 命名规范

无规矩不成方圆，编程也一样,程序中的包命名，类命名，接口命名，方法命名，变量命名，常类命名，抽象类命名，异常类命名以及扩展类命名等。合适的命名， 可以体现出元素的特征、职责 ，以及元素之间的差异性和协同性。

### 包命名规范

包(Package)的作用是将功能相似或相关的类或者接口进行分组管理，便于类的定位和查找，同时也可以使用包来避免类名的冲突和访问控制，使代码更容易维护。通常，包名使用小写英文字母进行命名，并使用“.”进行分割，每个被分割的单元只能包含一个名词。

一般地，包命名常采用顶级域名作为前缀，例如com，net，org，edu，gov，cn，io等，随后紧跟公司/组织/个人名称以及功能模块名称。

```
package org.springframework.boot.autoconfigure.cloud
package org.springframework.boot.util
package org.hibernate.action
package org.hibernate.cfg
package com.alibaba.druid
package com.alibaba.druid.filter
package com.alibaba.nacos.client.config
package com.ramostear.blog.web
```

### 类命名规范

类(Class)通常采用名词进行命名，且首字母大写，如果一个类名包含两个以上名词，建议使用驼峰命名(Camel-Case)法书写类名,每个名词首字母也应该大写。一般地，类名的书写尽量使其保持简单和描述的完整性，因此在书写类名时不建议使用缩写。

```
public class UserDTO{

}
class EmployeeService{
    
}
class StudentDAO{
    
}
class OrderItemEntity{
    
}
public class UserServiceImpl{
    
}
public class OrderItemController{
    
}
```



### 接口命名规范

接口(Interface)是一种表述某一类型对象动作的特殊类；简单来说，接口也是类(不太严谨)，所以，接口的名称的书写也应该符合类名书写规范，首字母应该大写，与普通类名不同的是，接口命名时通常采用形容词或动词来描述接口的动作行为。

```
public interface Closeable{
    
}
public interface Cloneable{
    
}
public interface Runnable{
    
}
public interface Comparable<T>{
    
}
public interface CompletionService<V>{
    
}
public interface Iterable<T>{
    
}
public interface EventListener{
    
}
```

### 接口实现类

为了便于阅读，在通常情况下，建议接口实现类使用“Impl作为后缀”，不建议使用大写的“I”作为接口前缀。

```
public interface OrderService{
    
}
public class OrderServiceImpl implements OrderService{
    
}
```

###  抽象类命名规范

抽象类(Abstract Class)是一种特殊的类，其命名与普通类的命名规范相当。一般地，为了将抽象类与普通类和接口做出区别，提高抽象类的可读性，在命名抽象类时，会以“Abstract”/“Base”作为类名的前缀。

```
public abstract class AbstractRepository<T>{
    
}
public abstract class AbstractController{
    
}
public abstract class BaseDao<T,ID>{
    
}
public abstract class AbstractCommonService<T>{
    
}
```

### 异常类命名规范

异常类(Exception Class)也是类的一种，但与普通类命名不同的是，异常类在命名时需要使用“Exception”作为其后缀。

在Java中还有另外一类异常类，它们属于系统异常，这一类异常类的命名使用“Error”作为其后缀，以区分Exception(编码，环境，操作等异常)。

```
public class FileNotFoundException{
    
}
public class UserAlreadyExistException{
    
}
public class TransactionException{
    
}
public class ClassNotFoundException{
    
}
public class IllegalArgumentException{
    
}
public class IndexOutOfBoundsException{
    
}
```

### 方法命名规范

方法(Method)命名时,其首字母应该小写，如果方法签名由多个单词组成，则从第二个单词起，使用驼峰命名法进行书写。一般地，在对方法进行命名时，通常采用动词/动词+名词的组合，下面是方法命名的一些常见示例。

**表述获取**

如果一个方法用于获取某个值，通常使用“get”作为其前缀。

```
public String getUserName(){
    
}
public List<Integer> getUserIds(){
    
}
public User getOne(){
    
}
```

**表述查询**

如果方法需要通过查询或筛选的方式获取某个数据，通常使用“find”/“query”作为其前缀。

```
public List<User> findOne(Integer id){
    
}
public List<Integer> findAll(){
    
}
public List<String> queryOrders(){
    
}
```

**表述条件**

如果一个方法需要一些条件参数，则可以使用“by”/“with”等字符作为方法名中条件的连接符。

```
public User findByUsername(String username){
    
}
public List<Integer> getUserIdsWithState(boolean state){
    
}
public List<User> findAllByUsernameOrderByIdDesc(String username){
    
}

```

**表述设置**

如果一个方法是要设置，插入，修改，删除等操作，应该将对应的动词(set,insert,update,delete)作为其名词的前缀。

```
public void setName(String name){
    
}
public User insert(User user){
    
}
public void update(User user){
    
}
public void clearAll(){
    
}
```

**其他规范**

如果一个方法用于获取某组数据的长度或数量，则该方法应该使用length或size命名；如果方法的返回值为布尔类型(Boolean)，则该方法应该使用“is”或”has”作为前缀；

如果方法用于将一种类型的数据转换为另一种数据数类型，则可以使用“to”作为前缀。

```
public long length(){
    
}
public int size(){
    
}
public boolean isOpen(){
    
}
public boolean isNotEmpty(){
    
}
public boolean hasLength(){
    
}
public Set<Integer> mapToSet(Map map){
    
}
public UserDto convertTo(User user){
    
}
public String toString(Object obj){
    
}
```



### 变量命名规范

变量(Variable)命名包括参数名称，成员变量和局部变量。变量命名通常以小写字母开头，如果变量名由多个单词构成，则从第二个单词起首字母需要大写，在变量命名过程中，不建议使用“_”作为前缀或者单词之间的分割符号。

```
private String nickName;
private String mobileNumber;
private Long id;
private String username;
private Long orderId;
private Long orderItemId;
```

### 常量命名规范

常量名称采用全部大写的英文单词书写，如果常量名称由多个单词组成，则单词之间统一使用“_”进行分割。

```
public static final String LOGIN_USER_SESSION_KEY = "current_login_user";
public static final int MAX_AGE_VALUE = 120;
public static final int DEFAULT_PAGE_NO = 1;
public static final long MAX_PAGE_SIZE = 1000;
public static final boolean HAS_LICENSE = false;
public static final boolean IS_CHECKED = false;
```

### 枚举命名规范

枚举(Enum)类是一种特殊的类，其命名规范遵循普通类的命名约束条件，首字母大写，采用驼峰命名法；枚举类中定义的值的名称遵循常量的命名规范，且枚举值的名称需要与类名有一定的关联性。

```
public enum Color{
    RED,YELLOW,BLUE,GREEN,WHITE;
}
public enum PhysicalSize{
    TINY,SMALL,MEDIUM,LARGE,HUGE,GIGANTIC;
}
```

### 泛型类

在书写泛型类时，通常做以下的约定：

- E表示Element，通常用在集合中；
- ID用于表示对象的唯一标识符类型
- T表示Type(类型)，通常指代类；
- K表示Key(键),通常用于Map中；
- V表示Value(值),通常用于Map中，与K结对出现；
- N表示Number,通常用于表示数值类型；
- ？表示不确定的Java类型；
- X用于表示异常；
- U,S表示任意的类型。

### 测试类和测试方法

测试类采用被测试业务模块名/被测试接口/被测试类+“Test”的方法进行书写，测试类中的测试函数采用“test”+用例操作_状态的组合方式进行书写。

```
public class UserServiceTest{

    public void testFindByUsernameAndPassword(){
        
    }

    public void testUsernameExist_notExist(){
        
    }

    public void testDeleteById_isOk(){
        
    }
}
```

