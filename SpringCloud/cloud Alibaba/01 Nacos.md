# 一、概述

> 服务注册与配置中心
>
> Nacos = Naming+Configuration+Service

- 就是注册中心和配置中心的组合。
- 相当于Cloud中，Eureka + Config + Bus

# 二、nacos环境安装

1. 打开Github https://github.com/alibaba/nacos/releases/tag/1.4.5

   ![image-20230609124806214](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609124806214.png)

   2. 下载完成后，解压运行即可

   ![image-20230609125520677](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609125520677.png)



# 三、服务注册与配置中心

## 1. Nacos注册中心演示

### 1.1 服务提供者注册

### 1.2 消费者这注册





为什么Nacos默认支持负载均衡?





### 2. 注册中心比较

  在Nacos具体支持AP、CP切换；

![image-20230602130543294](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230602130543294.png)

模式切换: 

![image-20230602130749997](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230602130749997.png)



## 2. 配置中心

nacos 配置中心，解决了SpringCloud 当中的配置服务还需要整合 Config-Bus 的痛点。提供了一系列解决方案，并且使用Dataid进行管理

- Dataid组成 - prefix-dev.ex

![image-20230609132145978](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609132145978.png)

### 2.1 基础配置



### 2.2 分类配置

  在实际的生产环境当中，往往是多环境，多项目的，比如外包、本地、远程、开发、生产、测试等等环境，不同环境需求的配置也是各不相同的，这就需要nacos中分类配置功能。

- nameSpace: 区分环境
- group和dataId区分两个目标对象

![image-20230609133418070](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609133418070.png)

![image-20230609133034309](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609133034309.png)

---

1. DataId 配置

active

2. Group 分组方案

![image-20230609135556276](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609135556276.png)





3. NameSpace 空间方案

![image-20230609135815021](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609135815021.png)





---

# 四、集群和持久化配置

  默认的Nacos使用嵌入式数据库实现数据存储，当启用默认配置的多个nacos节点，数据存储存在一致性问题，为了解决这一个问题，Nacos采用了集中式存储的方式来支持集群化部署，目前只支持Mysql存储。

## 1. 概述

### 1. 1 集群

![image-20230609152833143](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609152833143.png)

![image-20230609141129076](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609141129076.png)

### 1.2 持久化配置

  nacos当中自身带着嵌入式数据库 Derby , 现在我们需要做的就是从 derby 到 Mysql的配置。



# 2. 使用

1. Linux 中 Nacos 完成数据库配置



2. application.properties 配置数据源

  



3. Linux 服务器当中 Nacos 集群的配置 cluster.conf

IP 当中必须是 hostname -i 命令出来的IP

![image-20230609150324680](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609150324680.png)



4. 编辑startup.sh脚本

修改表示在使用启动命令的时候，可以添加 -P 参数区分端口

![image-20230609150638412](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609150638412.png)

![image-20230609151208867](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609151208867.png)

5. Naginx 配置

![image-20230609152302784](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609152302784.png)



![image-20230609152642750](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230609152642750.png)

6. 启动测试
