# 一、概念

## 1. Ribbon

  Ribbon是实现一套客户端，负载均衡的工具，简单的说，ribbon是一个开源项目，主要提供给客户端软件负载均衡算法和服务调用。

> 负载均衡和服务调用的提供者

  主要用于:

- 负载均衡
  - 将用户的请求平均到分配多个微服务当中，达到服务HA
  - 本地负载均衡: 会在注册中心上的列表进行负载均衡又称为进程内负载均衡

- 服务调用

对于负载均衡有很多种实现方式，例如我们之前学到过的Nginx的负载均衡，那么**Ribbon本地负载均衡**和**Nginx服务端**负载均衡有什么区别呢？

- Nginx服务器负载均衡负责的是接受客户端请求，然后由Nginx实现转发请求，即负载均衡由服务端进行实现
- Ribbon本地负载均衡，在调用微服务接口时候，会在注册中心将服务列表缓存到JVM本地，从而实现本地实现RPC调用远程服务。

> 最后简单总结，Ribbon就是负载均衡+RestTemplate

![image-20230512143110950](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230512143110950.png)



# 二、RT使用	

  在我们使用，不需要引如Ribbon依赖，*因为Eureka已经中有使用Ribbon了*；

- Entity: 返回详细的对象
  - 响应信息
  - 响应码...
- Object： 可以直接理解为JSON







# 三、负载均衡规则

IRule规则

- 轮询
- 随机
- 重试
- 轮询的扩展，轮询看响应速度
- 过滤故障实例，选择并发量最小的服务
- 默认规则

![image-20230512151716247](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230512151716247.png)

## 1. 规则替换

  注意配置规则！！

>  ***自定义配置类不能放在@ComponetScan所扫描的包和子包下，否则配置会被所有Ribbon客户端共享，达不到特殊定制化的目的***！

1. 远离ComponetScan创建包

创建myrule包;

![image-20230523191133311](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230523191133311.png)

2. 添加MySelfRule规则类

```java
@Configuration
public class MyIRule {
    /**
     * 开启随机轮询
     * @return
     */
    @Bean
    public IRule iRule(){
        return new RandomRule();
    }
}
```

3. 主启动类添加@RibbonClient

![image-20230523191322049](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230523191322049.png)

> 注意一个坑点，CLOUD-PAYMENT-SERVICE一定要大写，否则自定义规则无法启动。

---



## 2. 负载均衡轮询

### 原理

  负载均衡算法: **rest接口第几次请求%集群总数=实际调用服务器的位置下标**；

![image-20230523212333495](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230523212333495.png)

> JUC(CAS && 自旋锁);

### 手写源码

等待...

