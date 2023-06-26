# 一、概述

> 屏蔽底层消息中间件的差异，降低切换成本，统一消息的编程模型。降低切换维护和开发难度。

## 1. SpringCloudStream?

  官方定义是一个构建消息驱动的微服务框架。

 应用程序通过Inputs和outputs来与Spring Cloud Stream中Binder对象交互，通过我们配置来bingding，而Stream的binder对象负责与消息中间件交互，我们只需要搞清楚如何和Stream交互就可以了。

三大概念: 

- 发布-订阅
- 消费组
- 分区

> 目前仅仅支持RabbitMQ、Kafka



## 2. Stream设计思想

   ### 2.1 标准MQ与Stream

  在标准的MQ当中，生产者和消费者靠消息来传递信息内容，消息必须要走特定的通道.

![image-20230601165332952](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601165332952.png)

消息中间件的差异，导致我们实际项目中一些困扰和问题，我们如果因为业务需求，需要从一种消息中间件切换到另一种消息中间件，这种操作无疑是灾难性的。这时候Stream就为我们提供了一种解耦方式。

- 通过应用程序提供一个统一的Channel通道，使得应用程序不需要再考虑具体消息中间件的讨论
- Binder对象- INPUT -OUTPUT

### 2.2 Binder

  在没有绑定器概念之前，我们的SpringBoot项目需要直接和消息中间件打交道，而消息中间件各有各的差异，实现细节也更不相同；而通过定义统一绑定器，让应用程序和消息中间件之间隔离，引入Binder，使得微服务开发与消息中间件高度解耦，从而微服务只需要关注业务本身。

![image-20230601171256912](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601171256912.png)

> 通过Binder绑定器，实现应用和消息中间件之间的解耦

## 3. Stream流程

![image-20230601171916469](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601171916469.png)

- Binder
- Channel
- source \ sink

# 二、实操

## 1. 常用注解

![image-20230601172040751](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601172040751.png)

## 2. 案例

前置条件： MQ环境征程; 三个子模块工程

![image-20230601172211030](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601172211030.png)

### 1. 生产者消息驱动

### 2. 消费者消息驱动



### 3. 分组消费

重复消费问题的出现。



![image-20230601225114399](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601225114399.png)

> 在不同组中，产生重复消费
>
> 在同组当中，会发生竞争关系，只有其中一个消费

![image-20230601230141052](E:/%E5%B7%A5%E4%BD%9C/%E7%A7%81%E4%BA%BA%E7%9F%A5%E8%AF%86%E5%BA%93/image-20230601230141052.png)

分组方式: 

- YAML goup：

实现轮询分组



> 统一组多个实例，只有一个实例能拿到消息；

导致原因:

### 4. 持久化

  消息错过。加个group就可以
