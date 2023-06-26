> 等待填坑

#  一、概述

>   Zookeeper 从设计模式角度来理解: 是一个基于观察者模式设计的分布式服务管理框架，他负责存储和管理大家关心的数据；然后接受观察者的注册，一旦数据发生状态变化，Zookeeper 就会负责通过已经注册的观察者做出反应。

Zookeeper = 文件系统+通知机制

## 1. 特点

1. Zookeeper: 一个Leader和多个Follower组成的集群。

![image-20221113213420128](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221113213420128.png)

2. Zookeeper只要有半数的节点存活，集群就能够正常进行服务。适合奇数台服务。
3. 全局数据一致: 每个Server保存一份相同的数据副本
4. 更新请求顺序执行。FCFS
5. 数据更新原子性
6. 实时性: 一定范围，客户端能够读取到最新的数据。

## 2. 数据结构

  Zookeeper的数据模型结构也是树形结构，每个节点作为一个ZNode，每一个节点默认存储1MB，ZNode可以通过**路径唯一标识。**

## 3. 应用场景

  Zookeeper提供以下应用场景

- 统一命名服务
- 统一配置管理
- 统一集群管理
- 服务器节点动态上下线
- 软件负载均衡

### 3.1 统一命名服务

  在分布式服务当中经常需要对应用或者微服务进行统一命名。

  在用户视角，服务器是透明的，只需要记住服务名称(域名)即可。

![image-20221113214300640](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221113214300640.png)

> 分布式服务当中一个服务名称，对应着多个微服务副本。

## 3.2 统一配置管理

1. 分布式环境当中，**配置文件同步**
2. 配置管理由Zookeeper实现

![image-20221113215124305](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221113215124305.png)

### 3.2 统一集群管理

1. 分布式环境中，实时掌握每个节点的状态是必要

2. 可以实时监控节点状态变化
   1. 将节点信息写入Zookeeper成为ZNode
   2. 监听Znode
   
   
   
   ![image-20221114194656431](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114194656431.png)
   
   

### 3.3 服务器动态上下线

  客户端能够洞察到服务器上下线的变化。

### 3.4 软负载均衡

  在Zookeeper中记录每台服务器的访问次数，让访问数最少的服务器处理用户的客户端请求。

![image-20221114194939733](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114194939733.png)

> nginx也可以实现软负载均衡



## 4. 下载解安装

https://zookeeper.apache.org/releases.html

### 4.1 本地模式安装

1. 安装zookeeper需要安装JDK，

2. 将zookeeper放入到linux当中,解压和安装它

```shell
# tar -zxvf apache-zookeeper-3.7.1-bin\ \(1\).tar.gz -C /opt/moulde/
```

3. 修改名称

```shell
 mv apache-zookeeper-3.7.1-bin/ ./zookeeper-3.7.1
```

![image-20221114202111878](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114202111878.png)

4. 对zookeeper进行配置
   - 修改文件名称
   - 修改存储路径

![image-20221114202553314](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114202553314.png)

5. 启动服务

```shell
./zkServer start --启动服务端

./zkCli.sh -- 启动客户端
```



![image-20221114203136315](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114203136315.png)

![image-20221114203337574](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114203337574.png)

![image-20221114203500759](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114203500759.png)

> 基本操作以上

### 4.2集群安装

1. 集群规划
2. 解压安装
3. 配置服务器编号

  在zkData目录下，创建MYID文件，写上数字标识

4. 配置zoo配置文件

   1. 重命名
   2. 修改存储路径
   3. 添加配置

   ![image-20221114211937729](C:\Users\JaneOnly\AppData\Roaming\Typora\typora-user-images\image-20221114211937729.png)

```shell
server.A = B:C:D
# A是服务标识 1,2,3
# B是服务器地址
# C是Follower与leader的交换信息端口
# D是选举出新的Leader
```

5. 集群启动服务**

```shell
./zkServer.sh #服务启动，需要启动一半以上的服务器。
```

## 5. 操作命令与细节

### 5.1 基本操作命令

```shell
# 启动zookeeper服务
# 查看zookeeper服务状态
# 启动zookeeper客户端
# 退出zookeeper客户端
# 查看进程是否启动
# 停止zookeeper
```

### 5.3配置参数细节

```properties
# The number of milliseconds of each tick
# 通过心跳
tickTime=2000
# The number of ticks that the initial 
# synchronization phase can take
# 初始化时限
initLimit=10
# The number of ticks that can pass between 
# sending a request and getting an acknowledgement
# 同步通信时限
syncLimit=5
# the directory where the snapshot is stored.
# do not use /tmp for storage, /tmp here is just 
# example sakes.
# 存储zookeeper数据
dataDir=/opt/moulde/zkData
# the port at which the clients will connect
# 客户端连接端口号
clientPort=2181

```





> 等待填坑

