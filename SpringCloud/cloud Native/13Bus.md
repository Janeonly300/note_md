# 一、概述

![image-20230529115526114](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529115526114.png)  

> BUS 配置 Config 可以实现配置的动态刷新。

- Bus 支持两种消息代理，RabbitMQ、Kafka

  通知某个 Client ，继而影响全部。

![image-20230527175702053](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527175702053.png)

 

  



Bus 是能管理和传播分布式系统之间的消息，像一个分布式执行器，可以用于广播状态更改，事件推送等，也可以当做微服务之间的通信通道。

- 总线: 使用轻量级的消息代理，构建公用消息主题，该主题中产生消息会被所有实例监听和消费，所以称之为消息总线。
- ***原理:  ConfigClient 监听 MQ 中同一个Topic（SpringCloud BUS），当一个服务刷新数据，会将这个信息放入Topic中，这样其他监听同一个 Topic 服务就能得到通知，更新自己的配置***。

![image-20230527175820610](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230527175820610.png)







---



### 安装MQ	

1. 安装erlang 21,3
2. 安装rabbitmq 3.7.14
3. 进入RQ 的sbin，输入rabbitmq-plugins enable rabbitmq_management

![image-20230529120141223](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529120141223.png)

4. 访问址是否成功: 15672

1. 输入默认密码 guest guest

![image-20230529120244376](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529120244376.png)

---

#  二、Bus使用

  是用 Bus 的前提是必须先要具备 Mq 环境;

注意: Bus有两种动态刷新配置服务的思想

1. 运维 refresh client (发送post请求)，其他客户端节点跟着更新配置。
2. 运维 refresh Server（刷新ConfigServer），进而客户端从服务端获取配置

  针对以上两种实现思想方式，我们采用第二种; 因为第一种方式，使用客户端来通知其他客户端的行为，破坏了微服务的单一性，微服务只需要完成自己的业务就行了，不用再添加额外的刷新配置的职责。所以我们采取第二种方式。

## 1. 动态刷新全局广播通知

  实现一次通知，处处生效的效果。

### 1.1 服务端添加支持

1. 添加rabbitMQ支持

```xml
 <!--添加消息总线RabbitMQ支持-->
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-bus-amqp</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.cloud</groupId>
        <artifactId>spring-cloud-starter-config</artifactId>
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
```



1. yaml

```yaml
server:
  port: 3366

spring:
  application:
    name: config-client
  cloud:
    #Config客户端配置
    config:
      label: dev #分支名称
      name: config #配置文件名称
      profile: dev #读取后缀名称   上述3个综合：master分支上config-dev.yml的配置文件被读取http://config-3344.com:3344/master/config-dev.yml
      uri: http://localhost:3344 #配置中心地址
  rabbitmq:
    host: localhost
    port: 5672
    username: guest
    password: guest

#服务注册到eureka地址
eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka

# 暴露监控端点
management:
  endpoints:
    web:
      exposure:
        include: "bus-refresh"



```



启动 3355 3366 3344

### 1.3 测试

修改github文件为8，服务端更新成功，客户端未能更更新

![image-20230530175113870](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230530175113870.png)

发送post到服务端，实现一处更新处处更新;

![image-20230529130704680](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529130704680.png)



![image-20230530175402081](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230530175402081.png)





## 2. 动态单点通知

  只想要通知某个微服务进行配置更新，不去影响其他微服务的更新；

- dest: 需要更新的 **微服务名称:端口号**

![image-20230527200116974](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527200116974.png)

> 整合出现问题，待解决
>
> 



---

# 三、总结

![image-20230527200403998](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527200403998.png)
