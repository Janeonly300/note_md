# 一、引入

> Spring Cloud封装了netflix公司的Eureka模块来进行实现服务治理。
>
> 在传统的RPC远程调用框架中，管理每个服务服务之间依赖关系比较复杂，所以需要服务治理，管理服务之间的依赖。可以实现服务注册、调用、负载均衡、容错等技术。

![image-20230505220722812](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230505220722812.png)

## 1. 服务注册与发现

   ![image-20230505220806087](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230505220806087.png)

## 2. Eureka概念

  eureka包含两个不同的组件: Eureka Server和Eureka Client

### Eureka Server

  提供注册服务，各个服务节点可以通过配置启动，会在Eureka Server中注册

### Eureka Client

 是一个Java客户端，简化和Eureka Server的交互，客户端同时具备内置的、是用轮询的负载算法，来检测服务是否还活着。默认每个心跳周期为30s,若EurekaServer在三个周期内没收到某个服务心跳，则会将服务注册表中这个服务节点移除。

## 3. Eureka原理







# 二、eureka使用

## 1. 单点模式

1. 建立eureka的ServerMoudle

   ![image-20230505222625938](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230505222625938.png)

2. 编写pom文件

   ```xml
    <dependencies>
           <!-- 服务注册中心的服务端 eureka-server -->
           <!-- https://mvnrepository.com/artifact/org.springframework.cloud/spring-cloud-starter-eureka-server -->
           <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-server</artifactId>
           </dependency>
   
           <!--        <dependency>-->
           <!--            <groupId>com.atguigu.springcloud</groupId>-->
           <!--            <artifactId>cloud-api-commons</artifactId>-->
           <!--            <version>${project.version}</version>-->
           <!--        </dependency>-->
   
           <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-web</artifactId>
           </dependency>
   
           <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-web  -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-actuator</artifactId>
           </dependency>
   
           <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-devtools -->
           <!--        <dependency>-->
           <!--            <groupId>org.springframework.boot</groupId>-->
           <!--            <artifactId>spring-boot-devtools</artifactId>-->
           <!--            <scope>runtime</scope>-->
           <!--            <optional>true</optional>-->
           <!--        </dependency>-->
   
           <!-- https://mvnrepository.com/artifact/org.projectlombok/lombok -->
           <!--        <dependency>-->
           <!--            <groupId>org.projectlombok</groupId>-->
           <!--            <artifactId>lombok</artifactId>-->
           <!--        </dependency>-->
   
           <!-- https://mvnrepository.com/artifact/org.springframework.boot/spring-boot-starter-test -->
           <dependency>
               <groupId>org.springframework.boot</groupId>
               <artifactId>spring-boot-starter-test</artifactId>
               <scope>test</scope>
           </dependency>
           <dependency>
               <groupId>junit</groupId>
               <artifactId>junit</artifactId>
           </dependency>
       </dependencies>
   ```

   

3. 配置yaml文件

   ```yaml
   server:
     port: 7001
   eureka:
     instance:
       hostname: localhost
     client:
       serviceUrl:
       	#交互依赖该地址
         defaultZone: http://${eureka.instance.hostname}:${server.port}/eureka
        #证明自己是注册中心服务
       registerWithEureka: false
    	# 不进行注册
   	 fetchRegistry: false
   ```
   
   
   
4. 添加注解

   开启eureka服务。

   ```java
   @SpringBootApplication
   @EnableEurekaServer
   public class EurekaApplication {
       public static void main(String[] args) {
           SpringApplication.run(EurekaApplication.class);
       }
   }
   ```

   显示以下网页则说明服务启动成功。

   ![image-20230505222912158](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230505222912158.png)

   ### 1. 消费者服务注册

   1. 引入Eureka-client的依赖

   ```xml
     <dependency>
               <groupId>org.springframework.cloud</groupId>
               <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
           </dependency>
   
   ```

   

   1. 配置yaml文件

   ```yaml
   eureka:
     client:
       register-with-eureka: true    #表示不向注册中心注册自己
       fetch-registry: true   #表示自己就是注册中心，职责是维护服务实例，并不需要去检索服务
       service-url:
         #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
         defaultZone: http://localhost:7001/eureka
   ```

   

   1. 在主启动类添加注解启动

   

   ```java
   @SpringBootApplication
   @EnableEurekaClient
   public class OrderMainApplication {
       public static void main(String[] args) {
     	SpringApplication.run(OrderMainApplication.class);
       }
   }
   ```

   ![image-20230505224920437](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230505224920437.png)

   > 支付模块同以上步骤。

---



## 2、集群模式

![image-20230506221812246](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230506221812246.png)

微服务PRC远程服务调用当中，最核心的就是**高可用**！

> 注册服务中心，让其互相注册。互相守望。

1、 修改host文件，添加域名映射本机

```
######### springCloud2022.5.6 ##########
127.0.0.1 eureka7001.com
127.0.0.1 eureka7002.com
```

2、 创建两个服务注册中心微服务,两个服务实现互相调用

![image-20221111234237970](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221111234237970.png)

```yaml
#服务一
server:
  port: 7001
eureka:
  instance:
    hostname: eureka7001.com
  server:
    responseCacheUpdateIntervalMs: 3000
    responseCacheAutoExpirationInSeconds: 180
    evictionIntervalTimerInMs: 3000
  client:
    serviceUrl:
      defaultZone: http://eureka7002.com:7002/eureka
    healthcheck:
      enabled: true
    registerWithEureka: false
    fetchRegistry: false

# 服务二
server:
  port: 7002
eureka:
  instance:
    hostname: eureka7002.com
  server:
    responseCacheUpdateIntervalMs: 3000
    responseCacheAutoExpirationInSeconds: 180
    evictionIntervalTimerInMs: 3000
  client:
    serviceUrl:
      defaultZone: http://eureka7001.com:7001/eureka
    healthcheck:
      enabled: true
    registerWithEureka: false
    fetchRegistry: false

# 注册集群
server:
  port: 8001
spring:
  application:
    name: cloud-pri-service
  datasource:
    url: jdbc:mysql://localhost:3306/springcloud?useUnicode=true&characterEncoding=utf-8&useSSL=false&serverTimezone=UTC
    driver-class-name: com.mysql.jdbc.Driver
    username: root
    password: root
  devtools:
    restart:
      enabled: true

mybatis:
  type-aliases-package: com.atjianyi.entites #配置别名
  mapper-locations: classpath:com/atjianyi/mapper/xml/*.xml #配置mapper映射文件
  configuration:
    map-underscore-to-camel-case: true #配置文件

eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
# 注册测服务名称相同
server:
  port: 8002
eureka:
  client:
    register-with-eureka: true
    fetch-registry: true
    service-url:
      defaultZone: http://eureka7001.com:7001/eureka,http://eureka7002.com:7002/eureka
     #添加主机名称
   instance:
     instance-id: payment8002
     prefer-ip-address: true #显示IP
```

3. 启动服务，就可以访问eureka服务查看注册微服务。

![image-20230506225134757](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230506225134757.png)



4. 配置支付微服务集群

   以上步骤我们配置Eureka Server的多个集群服务，那么下面，我们就开始配置支付微服务的集群配置 ；开通8001 和8002服务

![image-20230506225541151](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230506225541151.png)

![image-20230506235208844](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230506235208844.png)

> 支付集群配置不同得端口即可。

5. 消费者调用支付集群

  先开启RestTemplate当中负载均衡模式，后修改调用地址为服务名称即可。无需关心端口和地址；

![image-20230506235641172](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230506235641172.png)

![image-20230506235946881](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230506235946881.png)



  最后，我们可以完善以下acturator的监控信息

![image-20230509201108749](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230509201108749.png)

---

## 3. 服务发现

   当我们想Eureka服务注册信息后，我们可以通过服务发现，来获取某个微服务的具体信息。

### 使用

1. 修改服务提供者的主启动类，开启服务发现

![image-20230509202819713](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230509202819713.png)

2. 在controller当中注入Dis

![image-20230509222051420](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230509222051420.png)

> 然而eureka已经停止更新了，不用学了 hhhh

## 4. Eureka自我保护

  Eureka在CAP理论当中，选择了AP，也就是在P的前提下保证A(可用性)，这种保护机制就是为了防止Client正常运行，但是与EurekaServer网络不通的情况，Server就不会立刻将Client移除。

![image-20230509222951879](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230509222951879.png)

禁用自我保护模式

Server端在2s收不到心跳，删除client记录

![image-20230509223626285](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230509223626285.png)

Client端每1s发送心跳

![image-20230509223809996](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230509223809996.png)



---

