# 一、概念

> hystrix停止更新，理念优秀。

  分布式系统面临的问题: 

- 对于复杂的分布式体系，有数十个依赖，依赖不可避免的错误。

- 服务会出现雪崩，**高可用受到破坏**。

  Hystrix就是用于解决分布式系统延迟和容错的开源库。

- 保证在一个依赖出现问题，不会导致整体的服务失败，避免级联故障，以提高分布式系统的弹性

- 如果出现错误，向调用方抛出备选FallBack



## 1. 功能

- 服务降级
- 服务熔断
- 接近实时监控
- 服务限流
- 服务隔离

> 停止更新进入维护

## 2. 重要理念

### 服务降级

  服务器出现问题时候，不让客户端持续等待，立刻返回一个友好的提示。**fallback**.

  以下原因会导致服务降级.

- 程序运行异常
- 服务熔断触发降级
- 线程池和信号量打满
- 程序超时

### 服务熔断

  当访问达到最大访问(简短可以理解为保险丝)，直接拒绝访问，然后调用服务降级方法，返回友好提示。

> 降级-熔断-恢复

### 服务限流

  秒杀高并发的操作，严禁一窝蜂过来拥挤，有序进行。 



---



# 二、Hystrix案例

> Hystrix一般用于消费端,也就是调用端;



## 1. 服务降级

什么时候开启服务降级呢？

1. 当被调用服务超时
2. 当被调用服务Down机
3. 当调用这等待时间小于服务端的处理时间



###  1.1 创建服务端

```xml
 <dependencies>
        <!--新增hystrix-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix</artifactId>
        </dependency>


        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>

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





1. 创建mould 8001端口，且注册进入eureka

![image-20230527222354845](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527222354845.png)

2. 在业务当中添加两个方法： 方便我们后续进行服务降级和熔断时候对比。
   - ok模拟正常业务
   - timeout模拟长流程业务员

![image-20230527222443058](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527222443058.png)



### 1.2 添加客户端

  出现客户端相应缓慢，8001同一层次的其他接口被困死，因为在tomcat线程池当中默认只有10个线程。

  客户端服务的核心就是调用远程服务。

![image-20230527223817593](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527223817593.png)

使用Jemeter测试，压测timeout方法，发现OK方法也出现了问题，相应变慢，我们应该怎么来改善这一问题，这就引入了我们下面要说的Hystrix;



### 1.3 Hystrix解决

- 超时导致服务器变慢
- 出错(程序运行出错)

解决方案: 

- 对方超时，必须有服务降级
- 对方down机，必须服务降级
- 对方OK，调用者自己出故障，自己处理降级

```@HystrixCommand(fallbackMethod)```

```EnableCirutBreaker```



#### 1.3.1 提供端降级

1. 在 Timeout 业务当中添加注解

![image-20230527132410536](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527132410536.png)

```java
  @Override
    @HystrixCommand(fallbackMethod = "timeoutHandler" ,commandProperties = {
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000") //表示业务处理若超过 3s,则降级，调用方法。
    })
    public String timeout(String id ) {
        int tmm = 3*1000;
        try {
            Thread.sleep(tmm);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "payment-timeout"+"current Thread "+Thread.currentThread().getId()+"   cID="+id;
    }

    /**
     * 降级回调方法
     * @param id
     * @return
     */
    public String timeoutHandler(String id){
        return "payment-timeout-handler"+"current Thread "+Thread.currentThread().getId()+"   cID="+id;
    }
```

2. 主启动类当中激活  @EnableCircuitBreaker

```java
@SpringBootApplication
@EnableEurekaClient
@EnableCircuitBreaker
public class HystrixPaymentApplication {
    public static void main(String[] args) {
        SpringApplication.run(HystrixPaymentApplication.class,args);
    }
}
```

测试结果

  由于配置了hystrix容忍 3s 的超时，实际业务需要 5s 所以这里服务进行了降级处理。

![image-20230527225446282](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527225446282.png)

  现在我们把 timeout() 代码改成如下，在进行测试:  结果显示降级并且调用到了降级方法。

```java
 @Override
    @HystrixCommand(fallbackMethod = "timeoutHandler" ,commandProperties = {
            @HystrixProperty(name="execution.isolation.thread.timeoutInMilliseconds",value = "3000") //表示业务处理若超过 3s,则降级，调用方法。
    })
    public String timeout(String id ) {
        //直接报错，观察是否会进行降级
        int t = 10/0;
        int tmm = 5*1000;
        try {
            Thread.sleep(tmm);
        } catch (InterruptedException e) {
            e.printStackTrace();
        }
        return "payment-timeout"+"current Thread "+Thread.currentThread().getId()+"   cID="+id;
    }
```

![image-20230527230433556](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527230433556.png)





---

#### 1.3.2 客户端降级

```yaml
server:
  port: 80
spring:
  application:
    name: cloud-hystrix-order
eureka:
  client:
    register-with-eureka: true    #表示不向注册中心注册自己
    fetch-registry: true   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
    service-url:
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://localhost:7001/eureka/
#      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka

feign:
  hystrix:
    enabled: true
ribbon:
  ReadTimeout: 5000
  ConnectionTimeout: 5000
```

  这里我们将主调用方法的超时容忍设为 1.5s, 而远程调用的长流程业务需要 3s完成，因此，测试一样会降级处理。

```java
 @GetMapping("consumer/payment/timeout/{id}")
    @HystrixCommand(fallbackMethod = "timeoutHandler",commandProperties = {
            @HystrixProperty(name = "execution.isolation.thread.timeoutInMilliseconds",value = "1500") //设置容忍度为1500
    })
    public R timeout(@PathVariable("id")String id){
        R ok = paymentClient.timeout(id);
        return ok;
    }
    public R timeoutHandler(String id){
        R<String> r = new R<>();
        r.setData("consumer-timeoutHandler  id="+id);
        return r ;
    }
```

![image-20230527232405879](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527232405879.png)



然而，上述代码的写法带来的两个问题

- 代码膨胀：每个方法都有一个hystrixConnmend,若有一百个接口，就得写一百个@hystrix...
- 代码混乱: 服务降级和正常业务逻辑进行了混淆

针对上面出现的两个问题，我们改写为下面的方法。

### 1.4 全局服务降级

全局服务降级配置，就是针对需要服务降级的进行通用配置 使用 @Defaultproperties()注解进行配置;

  再改写消费端如下，配置全局服务降级；

![image-20230527234054254](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527234054254.png)





![image-20230527234028877](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527234028877.png)



通过上一个配置，我们解决了代码膨胀得问题，接着我们最后写出一套完整通用得服务调用与降级的Demo。

### 1.5 究极服务降级

1. 在 client 接口当中直接指定降级处理类;

![image-20230527234707291](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527234707291.png)

2. 继承 client接口，并且实现方法。

![image-20230527234912706](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527234912706.png)

在测试的时候，将 服务端(被调用端)关闭，成功调用降级方法！

![image-20230527235212263](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527235212263.png)

> 这样，我们就解决了代码膨胀和代码混乱的问题;

---

## 2. 服务熔断

> 服务降级 -> 服务熔断 -> 链路恢复

![image-20230527141849197](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527141849197.png)

  断路器就是保险丝，熔断: 应对服务雪崩效应的链路保护机制，当检测节点微服务调用响应正常后，**恢复调用链路**。

- 熔点机制通过Hystrix实现
- 监控微服务调用的状况:
  - 5秒20次调用失败，就会启动熔断机制。

熔断是> https://martinfowler.com/bliki/CircuitBreaker.html

  ### 2.1 熔断原理

- 熔断打开: 请求不再进行调用当前服务

- 熔断关闭: 熔断关闭，不再对服务进行熔断

- 熔断半开: 部分请求根据规则调用当前服务，如果请求成功且符合规则，则关闭熔断

  设计断路器的三个重要参数; **快照时间窗口、请求总数阀值、错误百分比阀值**

- **快照时间窗口**: 统计请求和错误数据，默认为10s内

- **请求总数阀值**: 在快照时间窗内，必须满足请求总数才有资格熔断，默认为20;

- **错误百分比法制**: 当请求总数在快照时间内超过阀值，例如在10s内发生30次调用，有15次失败，则断路器打开。

![image-20230527142436114](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527142436114.png)

  当熔断开启后，直接调用fallback；



## 2.2 案例

1. 在 payment 业务当中，添加业务方法代码，以便于我们测试熔断机制。

```java
  @HystrixCommand(fallbackMethod = "paymentCircuitBreakHandler",commandProperties = {
            //开启熔断;
            @HystrixProperty(name = "circuitBreaker.enabled",value = "true"),
            //请求次数至少10次
            @HystrixProperty(name = "circuitBreaker.requestVolumeThreshold",value = "10"),
            //在10s中
            @HystrixProperty(name = "circuitBreaker.sleepWindowInMilliseconds",value = "10000"),
            //错误率 60%
            @HystrixProperty(name = "circuitBreaker.errorThresholdPercentage",value = "20")
    })
    public String paymentCirumentBreak(Integer id) {
        if(id<0){
            throw new RuntimeException("不能为负数");
        }
        //
        String rid = UUID.randomUUID().toString().replace("-","");
        return "调用成功: 流水号为: "+rid;
    }
    public String paymentCircuitBreakHandler(Integer id){
        return "调用失败: === 降级处理"+id;
    }
```





2. 在Controller当中调用Service，并打开浏览器进行测试；

   - 测试时候，传入负数参数连续调用n次，后在传入正数，会发现，正数返回的也是服务降级的内容，说明断路器Open;

   ![image-20230529091834445](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529091834445.png)

   - 再多次调用正数内容，熔断慢慢恢复到正常状态 closed;

   ![image-20230529091920222](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529091920222.png)

   > 上述熔断过程就是 closed -> open ->halfOpen -> closed 的过程

## 3. 服务监控

  Hystrix还提供了一个准实时的调用监控，以报表的方式或者图形化的方式展现给用户。SpringCloud提供了hystrixDashboard的整合，对监控内容转为可视化界面。

### 案例

1. 创建Module

![image-20230529093104348](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529093104348.png)

2. POM

```xml
   <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-hystrix-dashboard</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-actuator</artifactId>
  </dependency>
```



3. yaml

```yaml
Server.port=9001
```

4. 主类激活。

![image-20230529093756999](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529093756999.png)

5. 修改被监控的微服务

> 注意只要涉及到监控，就必须要添加 actuator 依赖

  再主类当中添加如下代码

![image-20230529094245345](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529094245345.png)

6. 访问localhost:9001/hystrix, 监控8001服务

  在hystrix监控栏目中输入被监控的地址: localhost:8001/hystrix.stream

![image-20230529095057567](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529095057567.png)



![image-20230529095122212](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529095122212.png)
