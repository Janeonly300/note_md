# 一、概述

## 1. 是什么？

> 约定 > 配置 > 编码

  Sentinel具有以下特征: 

- 丰富的应用场景
- 完美的实时监控
- 广泛的开源生态
- 完善的SPI扩展点

![image-20230612102138987](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230612102138987.png)

> sentinel 1.7就是hystrix的增强.

 ## 2. 环境搭建

  sentinel分为两个部分

- 核心库
- 控制台

1. 下载: https://github.com/alibaba/Sentinel/releases/tag/1.7.1
2. 运行命令
3. 访问界面 



# 二、案例

##  1. 初始化监控

## 2. 流控规则

### 2.1  基本介绍

  ![image-20230612104843301](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230612104843301.png)



### 2.2  流控模式

#### 直接

  测试QPS直接失败

  测试线程数直接失败

#### 关联

  当关联的资源达到阈值，限流自己

#### 链路



### 2.3 流控效果

#### 预热

#### 排队等待

---



## 3. 降级规则

> 注意在sentinel当中，没有半开熔断的概念

  ![image-20230612145728412](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230612145728412.png)

### 3.1 RT



### 3.2 异常比例



### 3.3 异常数

## 4. 热点规则



 ## 5. 系统规则

![image-20230612214649362](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230612214649362.png)



## 6. sentinel resource配置





![image-20230612220955412](https://raw.githubusercontent.com/Janeonly300/codeImg/main/img/image-20230612220955412.png)



# 三、服务熔断功能









# 四、规则持久化

