# 一、概述

  Feign是一个声明式的web服务的客户端，Feign就是参考Ribbon添加了注解+接口的绑定器。

- 我们封装一些客户端类来包装对其他服务的依赖调用。
- Feign让我们只需要创建一个接口+注解就能够实现操作。
- **Feign集成了Ribbon**

> 关于使用就是在接口添加特定注解就可以了。

## 为什么又要有Feign?

  前面在使用Ribbon和RestTemplate时候，利用RestTemplate对http请求封装处理，但在实际开发当中，微服务之间的依赖调用可能不止移除，往往一个接口可能被多个地方调用，所以我们通常会对接口封装一些客户端类来包装对服务的调用。简化了Ribbon的同时，自动封装调用客户端的开发量。

> 尽量面向接口编程 

Feign已经停止更新了，OpenFeign作为完全替换。

### ![image-20230523215246251](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230523215246251.png)





# 二、使用

## 1. OpenFeign入门使用

@FeignClient

1. 创建Modul 引入依赖

```xml
  <!--openfeign-->
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-openfeign</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <!-- 引入自己定义的api通用包 -->
        <dependency>
            <groupId>com.atjianyi</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>1.0-SNAPSHOT</version>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
        </dependency>

        <!--        <dependency>-->
        <!--            <groupId>org.springframework.boot</groupId>-->
        <!--            <artifactId>spring-boot-devtools</artifactId>-->
        <!--            <scope>runtime</scope>-->
        <!--            <optional>true</optional>-->
        <!--        </dependency>-->

        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

2. yaml

```yaml
server:
  port: 80
eureka:
  client:
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka/, http://eureka7002.com:7002/eureka/
    register-with-eureka: true 
```

2. 创建客户端接口

![image-20230524104552744](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230524104552744.png)

3. 在Controller类当中调用

![image-20230524104633448](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230524104633448.png)

4. 主类当中开启OpenFeign

![image-20230524104659270](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230524104659270.png)

## 2. 超时控制

  在微服务当中，消费者调用提供者时，当中提供者提供的业务有长流程业务时，如果没有超时控制，可能出现调用超市失败的情况。

  在Feign客户端当中，默认等待回调时间式 1s,若超过，则抛出一个错误，我们可以通过设置延长超时时间，来适应提供者可能的长流程业务。



- 调用一个长流程业务，但没修改超时控制: 

![image-20230524110017259](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230524110017259.png)

添加超时控制配置解决



![image-20230524110228683](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230524110228683.png)



## 3. 日志增强

  Open Feign的日志级别，越往下输出越多。

- NONE
- BASIC
- HEADERS
- FULL

1. 创建配置日志Bean

```java
@Configuration
public class FeginConfig {
    @Bean
    Logger.Level feignLoggerLever(){
        return Logger.Level.FULL;
    }
}
```

2. yaml日志监控

```yaml
logging:
  level:
    com.atjianyi.cloud.client.PaymentClient: debug
```

![image-20230524110632927](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230524110632927.png)