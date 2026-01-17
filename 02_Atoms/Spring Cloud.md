### spring组成部分
Spring Cloud = 多个 Spring Boot 应用 + 一套分布式治理组件

👉 **不是“随便多个 Spring Boot”**  
👉 而是 **“互相协作的多个 Spring Boot”**

### 分布式治理组件
服务注册与发现

- Nacos / Eureka
- 👉 解决：**我去哪找其他服务？**

 服务调用

- OpenFeign / RestTemplate
- 👉 解决：**怎么调用其他 Spring Boot**

 负载均衡

- LoadBalancer
- 👉 多实例时选哪一个？

 服务容错

- Sentinel / Resilience4j
- 👉 防止雪崩

 配置中心

- Nacos Config / Apollo
- 👉 配置不写死在 yml

网关

- Spring Cloud Gateway
- 👉 所有请求统一入口

在面试中这么说 👇

> Spring Cloud 本质上是以 **Spring Boot 作为服务载体**，  
> 通过 **注册中心、网关、配置中心等组件**，  
> 将多个独立的 Spring Boot 应用组织成一个分布式系统。


### 学习路径

1️⃣ **单体 Spring Boot（业务分包）** ← 你已经在这一步  
2️⃣ 拆一个模块 → 独立 Spring Boot  
3️⃣ 引入 Nacos  
4️⃣ 用 Feign 调服务  
5️⃣ 加 Gateway