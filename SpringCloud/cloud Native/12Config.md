# 一、概述

  由于分布式微服务的引入，每个微服务都需要有一个application.properties或者yaml文件，当微服务变得很多的时候，就会面临严重的配置问题。因此，**动态的配置管理成为必要**

- Config为微服务架构的微服务提供了中心化的外部配置支持，配置服务器能为各种不同微服务应用的所有环境提**供一个中心化的外部配置**。

  公有的配置放在Config当中，私有的配置放在各自微服务当中。

![image-20230527164233998](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527164233998.png)

**如何使用？**

  SpringCloud Config分为服务端和客户端两个部分。

- 服务端: 分布式配置中心，也是一个独立的微服务应用。
- 客户端：通过配置中心来管理应用资源业务相关的配置内容。

**能做什么用？**

- 集中管理配置文件

- 不同环境不同配置，动态化的配置更新，分环境部署/dev/test/prod/beata/release...

- 运行期间动态调整配置，不需要每个服务器当中改写配置文件，服务会向配置中心统一拉去自己的信息。

- 当配置变动，服务不需要重启就可以应用新的配置
- 配置信息以REST接口形式暴露

  默认使用Git来存储配置文件;

# 二、使用

## 1. Config Server端配置与测试

1. 在 GitHub 当中新建一个 Repository
2. 在本地新建 git 仓库 Clone 远程仓库
3. 建立多环境配置文件

![image-20230529105818320](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529105818320.png)

4. 新建模块 3344

   依赖

```xml
 <dependencies>

        <!--添加消息总线RabbitMQ支持-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-bus-amqp</artifactId>
        </dependency>

        <!--        Config服务端-->
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-config-server</artifactId>
        </dependency>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-netflix-eureka-client</artifactId>
        </dependency>
        <dependency>
            <groupId>com.atguigu.springcloud</groupId>
            <artifactId>cloud-api-commons</artifactId>
            <version>${project.version}</version>
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
    </dependencies>

```

yaml配置

```yaml
server:
  port: 3344

spring:
  application:
    name: cloud-config-center #注册进Eureka服务器的微服务名
  cloud:
    config:
      server:
        git:
          uri: https://github.com/Janeonly300/configserver.git #GitHub上面的git仓库名字
          search-paths: #搜索目录
            - configserver
      label: master #读取分支
      #启动成功后访问的路径 http://ip:3344/{label}/{application}-{profile}.yml 能访问的配置文件 就表示成功了

eureka:
  client:
    service-url:
      defaultZone: http://localhost:7001/eureka
```

```java
@SpringBootApplication
@EnableEurekaClient
@EnableConfigServer
public class ConfigServer7001 {
    public static void main(String[] args) {
        SpringApplication.run(ConfigServer7001.class,args);
    }
}
```



5. 启动访问 localhost:3344/{branch}/{application}-{profile}.yaml

![image-20230529112036320](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230529112036320.png)



### 配置读取规则

- name
- profiles
- label

推荐读取: 

![image-20230527170113804](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230527170113804.png)







## 2. Config客户端配置与测试

1. 新建 Config 客户端 3355

![image-20230529113745114](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529113745114.png)

1. 配置文件和依赖

xml

```xml
<dependencies>
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


</dependencies>

```



```java
server:
  port: 3355

spring:
  application:
    name: config-client
  cloud:
    config:
      label: dev  #分支名称
      name: application  #配置文件名称
      profile: dev  #读取后缀名称   上述三个综合http://localhost:3344/master/config-dev.yml
      uri: http://localhost:3344  #配置中心的地址
#服务注册到eureka地址
eureka:
  client:
    service-url:
      #设置与eureka server交互的地址查询服务和注册服务都需要依赖这个地址
      defaultZone: http://localhost:7001/eureka #单机版

```

2. 启动测试是否能够获取配置文件

![image-20230529114227656](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529114227656.png)





  随着而来出现一个问题，当我们在github上修改配置文件时，ConfigServer3344是能够及时的更新最新内容，而ConfigClient没有更新，这就涉及到动态刷新操作，下一小节就会解决这个问题。

### 配置动态刷新

1. 修改ConfigClient3355的yaml文件，暴露监控端口

```yaml
# 暴露监控端点 否则 curl -X POST "http://localhost:3355/actuator/refresh" 不可使用
management:
  endpoints:
    web:
      exposure:
        include: "*"
```



2. 在Controller当中添加 @RefreshScope 注解

![image-20230529114729788](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529114729788.png)



修改github文件，此时还是server端更新，client端没有更新

![image-20230529115059506](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529115059506.png)

3. 使用curl post请求 client3355暴露出来的端口,更新成功

```bash
http://localhost:3355/actuator/refresh
```

![image-20230529115148337](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230529115148337.png)



> 以上动态刷新操作避免了client需要重启才可以刷新配置文件的问题。但随之而来的又出现一个问题，那就是微服务当中不可能只有一个client，当出现多个client，我们怎么实现多个client同时刷新呢(排除脚本批量操作)?又怎么实现部分更新，部分保留的操作呢？ 这就涉及到下一章节要学习的 BUS 消息总线了；









