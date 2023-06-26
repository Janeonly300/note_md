# 一、概述

分布式事务的由来: 

  在微服务应用当中，需要将单体应用拆分为多个微服务应用，原来的N个模块拆分成N个独立的应用，分别使用不同的数据源；业务操作需要调用n个服务来完成，此时每个服务内部的数据一致性有本地保证，但全局的**数据一致性无法保证**。

## Seta术语

典型的分布式事务过程:  1+3

一个XID+三个组件模块：

- **全局唯一事务ID**

![image-20230617145354676](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230617145354676.png)



#### TC (Transaction Coordinator) - 事务协调者

维护全局和分支事务的状态，驱动全局事务提交或回滚。

#### TM (Transaction Manager) - 事务管理器

控制全局事务的边界，负责开启一个事务，并最终发起全局提交和全局回滚的决议

#### RM (Resource Manager) - 资源管理器

管理分支事务处理的资源，与TC交谈以注册分支事务和报告分支事务的状态，并驱动分支事务提交或回滚。

![image-20230617150323227](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230617150323227.png)

# 二、使用

![image-20230617151427031](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230617151427031.png)

## 1. 安装

1. 在官网当中安装



2. 修改file.conf

 自定义事务组名称+事务日志的默认存储模式DB+数据库连接信息

service模块

![image-20230626204835654](C:/Users/janeonly/AppData/Roaming/Typora/typora-user-images/image-20230626204835654.png)

store模块

 ![image-20230626205051049](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230626205051049.png)



4. 在Mysql当中建立Seta库



5. 修改Registry.conf文件

![image-20230626205333449](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230626205333449.png)



6. 启动nacos、再次启动setaServer



---

## 2. 案例

  ![image-20230626205719045](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230626205719045.png)

### 1. 业务库的准备

1. 创建业务数据库



2. 创建数据表



3.  建立每个数据库各自的回滚日志表



### 2. 业务微服务的准备





