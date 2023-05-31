### Spring Event 介绍

Spring Event（`Application Event`）其实就是一个观察者设计模式，一个 Bean 处理完成任务后希望通知其它 Bean 或者说一个 Bean 想观察监听另一个Bean 的行为。

要使用Event只要准备三个部分：

- 事件类：定义事件，继承`ApplicationEvent`的类成为一个事件类。
- 发布者：发布事件，通过`ApplicationEventPublisher`发布事件。
- 监听者：监听并处理事件，实现`ApplicationListener`接口或者使用`@EventListener`注解。

### 事件类

定义事件，继承 `ApplicationEvent` 的类成为一个事件类，只要继承`org.springframework.context.ApplicationEvent`，便是一个Spring Event类。一般我们会专门为一个类型的Event写一个抽象的事件类，作为该类型的所有事件的父类。

```
import lombok.Data;
import lombok.ToString;
import org.springframework.context.ApplicationEvent;

/**
 * 事件类：定义事件，继承ApplicationEvent的类成为一个事件类。
 *
 * @author wxh_work@163.com
 * @date 2022/6/28 15:53
 */
@Data
@ToString
public class OrderEvent extends ApplicationEvent {
    /** 该类型事件携带的信息 */
    private String orderId;
    public OrderEvent(Object source, String orderId) {
        super(source);
        this.orderId = orderId;
    }
}
```



### 发布者

发布者负责发布消息，Spring容器中默认的`ApplicationEventPublisher`是`AbstractApplicationContext`，同时`AbstractApplicationContext`也是`ApplicationContext`的一个子类，也就是说，Spring默认使用`AbstractApplicationContext`发布事件。

有三种实现方式：

1. 直接使用ApplicationEventPublisher(推荐)
2. 实现ApplicationEventPublisherAware接口(推荐)
3. 使用ApplicationContext#publishEvent(ApplicationEvent)发布

**`ApplicationContext`是`ApplicationEventPublisher`的一个实现，在有前面的两种方案之后，其实就不需要这个重复封装实现方案了。当然，你也可以直接使用`ApplicationContext`。**

```
import com.veganlefty.event.OrderEvent;
import lombok.RequiredArgsConstructor;
import lombok.extern.slf4j.Slf4j;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.context.ApplicationEventPublisher;
import org.springframework.stereotype.Service;

/**
 *
 *
 * @author wxh_work@163.com
 * @date 2022/6/28 16:02
 */
@Slf4j
@Service
@RequiredArgsConstructor
public class OrderService {
    /*
    发布者：发布事件，通过ApplicationEventPublisher发布事件。
    发布者负责发布消息，Spring容器中默认的ApplicationEventPublisher是AbstractApplicationContext，
    同时AbstractApplicationContext也是ApplicationContext的一个子类，
    也就是说，Spring默认使用AbstractApplicationContext发布事件。
    有三种实现方式:
    1：直接使用ApplicationEventPublisher(推荐)
    2：实现ApplicationEventPublisherAware接口(推荐)
    3：使用ApplicationContext#publishEvent(ApplicationEvent)发布
    */
    @Autowired
    private ApplicationEventPublisher publisher;

    public String buyOrder(String orderId) {
        long startTime = System.currentTimeMillis();
        //1.查询商品信息
        log.info("查询商品信息{}",orderId);
        //2.校验商品价格
        publisher.publishEvent(new OrderEvent(this,orderId));
        //3.购买成功，发送消息
        publisher.publishEvent(new MessageEvent(this,orderId));
        long endTime = System.currentTimeMillis();
        log.info("任务全部完成，总耗时：{}毫秒", endTime - startTime);
        return "商品下单成功";
    }
}
```



### 监听器

监听器负责接收和处理事件。

有两种实现方法：实现`ApplicationListener`接口或者使用`@EventListener`注解。

```
import com.veganlefty.event.OrderEvent;
import lombok.extern.slf4j.Slf4j;
import org.springframework.context.event.EventListener;
import org.springframework.stereotype.Component;

/**
 * description
 *
 * @author wxh_work@163.com
 * @date 2022/6/28 15:56
 */
@Slf4j
@Component
public class OrderListener {

    @EventListener
    public void onApplicationEvent(OrderEvent event) throws InterruptedException {
        String orderId = event.getOrderId();
        long startTime = System.currentTimeMillis();
        Thread.sleep(3000);
        long endTime = System.currentTimeMillis();
        log.info("{}:校验当前商品价格耗时:{}毫秒",orderId,(endTime-startTime));

    }
}
```

### 异步执行 

上述中，购买商品成功后，需要发送消息，这一步可以异步执行，减少接口请求等待时间

@Async实现异步，启动异步，启动类增加 `@EnableAsync` 注解

```
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.scheduling.annotation.EnableAsync;

@EnableAsync
@SpringBootApplication
public class SpringbootEventApplication {

  public static void main(String[] args) {
    SpringApplication.run(SpringbootEventApplication.class, args);
  }
}
```

`Listener` 类需要开启异步的方法增加 `@Async` 注解

```
import com.veganlefty.event.MessageEvent;
import lombok.extern.slf4j.Slf4j;
import org.springframework.context.event.EventListener;
import org.springframework.scheduling.annotation.Async;
import org.springframework.stereotype.Component;

/**
 * description
 *
 * @author wxh_work@163.com
 * @date 2022/6/28 17:37
 */
@Slf4j
@Component
public class MessageListener {

    @Async
    @EventListener
    public void sendMessage(MessageEvent event) throws InterruptedException {
        String orderId = event.getOrderId();
        long startTime = System.currentTimeMillis();
        log.info("开发发送短信");
        log.info("开发发送邮件");
        Thread.sleep(4000);
        long endTime = System.currentTimeMillis();
        log.info("{}：发送短信、邮件耗时：{}毫秒", orderId, (endTime - startTime));
    }
}
```



> https://donespeak.gitlab.io/posts/200411-spring-event/