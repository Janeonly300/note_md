

  

# 一、概述

![image-20230527150125935](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527150125935.png)

  GateWay是zuul的替代品，由于Zuul2.0迟迟没有出来，SpringCloud社区推出了gateWay网关来替代zuul1.x版本。提供了以下功能: 底层使用netty通讯

- 反向代理
- 鉴权

- 安全
- 监控、指标
- 限流

  GateWay具有以下特征: 基于Spring，PR，SpringBoot构建

- **动态路由**
- 集成hystrix断路器功能
- 路径重写
- 请求限流等功能
- **断言和过滤器**

## 1. 非阻塞异步模型

​    Getway是基于**异步非阻塞模型**上进行开发，性能更优秀。

- 阻塞处理模型就是: 当请求进入ServletContainer，它就会为之绑定一个线程，这种模型只适合用于并发不高的情况。Zuul1.x就是基于这么一个阻塞式处理模型。

  
  
  Servlet3.1之后有异步非阻塞的支持，webFlux就是基于这么个框架，他的核心就是基于Reactor相关的实现

![image-20221128170458657](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128170458657.png)

## 2. 三大概念



### 路由Route

  路由是构建网关的基本模块，由ID，和目标URI，和一系列断言和过滤器组成，如果断言为True则匹配该路由。

> 浅浅可以与Vue的路由概念相理解

### 断言Predicate

  断言就是Java8中Java.utils.function.Predicate，匹配Http请求中的内容，如果**请求与断言相互匹配**，则进行路由转发到微服务。

### 过滤Filter

  GatewayFilter的实例，使用过滤器，在请求被路由转发之前或者之后对请求进行修改

> like ServletFilter: beforeFilter doAction afterFilter

1. 客户端向GateWay请求，先通过网关中断言
2. 在进行n层过滤器
3. 最后根据目标uri访问对应的微服务

![image-20230527151751512](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527151751512.png)

![image-20230527152648037](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527152648037.png)

> 总结gateway的作用就是路由转发+执行过滤链





# 二、入门配置

## 1. 基础配置

### 1.1 Yaml方式

使用gatway网关，来访问8001的微服务

1. 建立Pom

```xml
<!--gateway-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
<!--eureka-client-->
<dependency>
    <groupId>org.springframework.cloud</groupId>
    <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
</dependency>
```

2. 修改YAML

  由于网关也是一种微服务，因此也需要注册进入服务注册中心；

```yaml
server:
  port: 9527

spring:
  application:
    name: cloud-gateway
  cloud:
    gateway:
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001          #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/circuitBreak/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          uri: http://localhost:8001          #匹配后提供服务的路由地址
          predicates:
            - Path=/payment/ok/**         # 断言，路径相匹配的进行路由

eureka:
  instance:
    hostname: cloud-gateway-service
  client: #服务提供者provider注册进eureka服务列表内
    service-url:
      register-with-eureka: true
      fetch-registry: true
      defaultZone: http://eureka7001.com:7001/eureka
```

3. 启动网关和8001微服务，使用9527端口进行访问; 

![image-20230529101512619](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529101512619.png)



---

### 1.2 硬编码方式配置路由

![image-20230529101903517](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529101903517.png)





## 2. 通过微服务实现动态路由

   通过上面的案例中，出现了两个问题

- 路由地址是硬编码方式
- 微服务中可能有多台机器，没有实现负载均衡

我们可以通过修改Yaml，实现动态路由来解决以上两个问题。

1. 开启动态路由
2. 将 uri 改成 lb: //{微服务名称}

```yaml
  cloud:
    gateway:
      routes:
        - id: payment_routh #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
         # uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://CLOUD-HYSTRIX-PAYMENT/
          predicates:
            - Path=/payment/circuitBreak/**         # 断言，路径相匹配的进行路由

        - id: payment_routh2 #payment_route    #路由的ID，没有固定规则但要求唯一，建议配合服务名
          #uri: http://localhost:8001          #匹配后提供服务的路由地址
          uri: lb://CLOUD-HYSTRIX-PAYMENT/
          predicates:
            - Path=/payment/ok/**         # 断言，路径相匹配的进行路由
      discovery:
        locator:
          enabled: true #开启识别动态路由
```

## 3. Predicate使用

​    在官方自带的断言有11种

![image-20230527160215091](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527160215091.png)

  以上各种RoutePredicateFactory都代表了HTTP请求中不同属性值，之间都可以进行组合，并且通过逻辑AND进行断言匹配。

### 3.1 After

  匹配时间: **ZonedDateTime**

![image-20221128211153223](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128211153223.png)

### 3.2 cookie

- 匹配是否有Cookie，若有，匹配成功，否则拒绝访问。
- ![image-20221128213100527](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128213100527.png)

![image-20221128213157753](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128213157753.png)

### 3.2 Header

- 匹配中检测是否有请求头，若有匹配成功，否则拒绝访问。

![image-20221128213331414](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128213331414.png)

![image-20221128213426307](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128213426307.png)

![image-20221128213459763](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221128213459763.png)

### 3.3 Method

- 匹配请求方式GETPost等等。

> predicate就是为了实现一组匹配规则，让请求过来找到对应的route进行处理。

## 4. Filter

  使用过滤器，可以在路由转发请求前，或者请求后，对请求进行一定的修改，也就是可以修改进入的HTTP请求和返回的HTTP相应，路由过滤器只能指定路由进行使用。

- **生命周期-pre|post**
- **种类-GetwayFilter|GlobalFilter**

根据官网文档，添加配置: 

  简单实例:

yaml

![image-20230527161656403](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527161656403.png)

### 自定义过滤器**

  可以进行全局日志记录，同一网关鉴权等..



1. 实现两个接口 GlobalFilter,Ordered

```java
@Component
public class MyLoagGatewayFilter implements GlobalFilter, Ordered {
    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        String uname = exchange.getRequest().getQueryParams().getFirst("uname");
        if(uname == null){
            exchange.getResponse().setStatusCode(HttpStatus.NOT_ACCEPTABLE);
            return exchange.getResponse().setComplete();
        }
        return chain.filter(exchange);
    }

    /**
     * 优先级
     * @return
     */
    @Override
    public int getOrder() {
        return 0;
    }
}

```

测试: 

不带uname访问:

![image-20230529103844592](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529103844592.png)





带着uname访问成功

![image-20230529103823335](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529103823335.png)



















1. 重写两个方法