# 一、概述

![image-20230601232245862](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601232245862.png)



![image-20230601232407348](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601232407348.png)



在微服务框架当中，一个由客户端发起的请求在后端系统中会经过多个不同的服务节点调用，协同产生最后的请求结果，每一请求都会形成一个复杂分布式调用链路，链路中的任何一环出现高延时或者错误都会引起失败。 所以，我们引入了Sleuth,进行链路追踪。

![image-20230601231700005](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601231700005.png)

总而言之: Sleuth提供一套完整分服务追踪的解决方案并且兼容ZKIN；

# 二、搭建

![image-20230601231957506](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601231957506.png)

1. 下载Zipkin 2.12.9.jar

![image-20230601232036002](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230601232036002.png)

2. 提供者模块
3. 消费者模块





---





