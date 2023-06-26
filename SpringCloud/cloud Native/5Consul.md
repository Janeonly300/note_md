# 一、概述

  Consul是一套开源分布式**服务发现**和**配置管理**的系统，使用Go语言开发，提供微服务中服务治理、配置中心、控制总线等功能。这些功能每个都可以单独使用。

- 服务发现

  支持HTTP和DNS两种发现方式

- 健康检测

  支持多种方式，进行校本化

- KV存储

  采用keyValue的存储方式

- 多数据中心

- 可视化Web页面

# 二、安装

  我们在官方下载安装即可，下载完成后，双击exe文件

![image-20230511112742293](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230511112742293.png)

```cmd
consul agent -dev #开发者模式启动
```

  访问localhost:8500

![image-20230511112836134](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230511112836134.png)



# 三 、注册进行Consul

## 1. 注册服务提供者

1. 新建Module8006
2. 添加pom
3. 修改yaml

4. 创建主启动类





## 2. 注册服务消费者

1. 创建服务消费者
2. 修改pom
3. 修改Yaml
4. 配置RestTemplateBean
5. 调用服务提供者。







---

