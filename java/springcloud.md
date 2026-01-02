单体应用、组件、模块、传统紧耦合、容错率低、应用模块、

## 系统架构

- SSM=》单体应用架构项目
    
- springboot=》垂直应用架构（前台+后台）
    
- 分布式架构
    
- SOA架构
    
- 微服务架构
    

### 单体应用架构：

- 一个应用，将所有功能代码都部署在一起就可以，用一个数据库，打个这样可以减少开发、部署和维护的成本。
    
- 一个电商系统，里面会包含很多用户管理，商品管理，订单管理，物流管理等等很多模块， 我们会把它们做成一个web项目，然后部署到一台tomcat服务器上。
    

> 打WAR包”是指将一个Java Web应用程序的所有资源、配置和代码文件打包成一个 **WAR（Web Application Archive）** 文件，用于部署在支持Java EE（Jakarta EE）规范的应用服务器（如Tomcat、Jetty或WildFly）上。
> 
> 经完成Java Web项目的开发，项目包含必要的目录结构：
> 
> MyWebApp/  
> ├── src/                  // 源代码目录  
> ├── WEB-INF/              // Web配置目录  
> │   ├── web.xml           // 必须的Web应用描述符  
> │   ├── classes/          // 编译后的类文件  
> │   └── lib/              // 第三方依赖的JAR文件  
> └── resources/            // 静态资源目录（HTML、CSS、JS等）

使用：企业宣传网、外包公司外包项目、学校学生学习

### 垂直应用架构

所谓的垂直应用架构，就是将原来的一个应用**拆成互不相干的几个应用**，以提升效率。比如我们可以将上面电商的单体应用拆分成： 电商系统(用户管理商品管理订单管理) 后台系统（用户管理订单管理客户管理) CMS系统(广告管理营销管理) 这样拆分完毕之后，一旦用户访问量变大，只需要增加电商系统的节点就可以了，而无需增加后台 和CMS的节点。

**优点：** 系统拆分实现了流量分担，解决了并发问题，而且可以针对不同模块进行优化和水扩展 个系统的问题不会影响到其他系统，提高容错率 **缺点：** 系统之间相互独立，无法进行相互调用 系统之间相互独立，会有重复的开发任务

### 分布式架构

当垂直应用越来越多，**重复的业务**代码就会越来越多。这时候，我们就思考可不可以将重复的代码抽取出来，做成统一的**业务层作为独立的_服务_**，然后由**前端控制层**调用不同的业务层服务呢？ 这就产生了新的分布式系统架构。它将把工程拆分成表现层和服务层两个部分，服务层中包含业务 逻辑。表现层只需要处理和页面的交互，业务逻辑都是调用服务层的服务来实现。

产生的分布式问题:

分布式缓存：redis

分布式锁：redis/zookeeper

分布式cision：redis

远程服务调用：dubbo、http、rest

负载均衡：njnix

分布式事务：

优点： 抽取公共的功能为服务层，提高代码复用性 **缺点：** 系统间耦合度变高，调用关系错综复杂，难以维护

### SOA架构（中心化架构）

在分布式架构下，当服务越来越多，容量的评估，小服务资源的浪费等问题逐渐显现，此时需增加 个_**调度中心**_对服务进行实时管理。此时，用于资源调度和治理中心（SOAServiceOrientedArchitecture）

优点： 使用治理中心（ESB\dubbo）解决了服务间调用关系的自动调节 缺点： 服务间会有依赖关系，一旦某个环节出错会影响较大（服务雪崩） 服务关系复杂，运维、测试部署困难

### 微服务架构（去中心化的 ）

微服务架构在某种程度上是面向服务的架构SOA继续发展的下一步，它更加强调服务的"彻底拆分"。

- 网关、注册中心是中心化的但不强行
    
- 数据库独立
    
- 分布式粒度很细
    

优点： 服务原子化拆分，独立打包、部署和升级，保证每个微服务清晰的任务划分，利于扩展 微服务之间采用Restful等轻量级http协议相互调用（相比SOA） 缺点： 分布式系统开发的技术成本高(容错、分布式事务等） 复杂性更高。各个微服务进行分布式独立部署，当进行模块调用的时候，分布式将会变得更加麻烦。

业务接口、上层轻量API接口 （ grpc）实现调用、

接口交互和调用：服务的注册和发现中心

微服务网关：前端调后端

API网关：调用方法粒度

## spring cloud alibaba :组件

- Nacos  
- Ribbon、LOadBalancer  
- Feign  
- Sentinel  
- Seata  
- Skywalking

spring cloud netflix

## 微服务概述

是一种架构风格

nacos管理微服务：注册中心，服务注入（用名字注入）

通讯实现服务之间的调用：springtemplate、httpclient、feign、restful、rcp、dubbo

客户端访问：网关gateway（微服务的大门）

容错功能：sentinel

排错功能：链路追踪sky walking

负载均衡：

在微服务架构中，服务注册是通过服务名称（服务名）作为唯一标识来实现的。服务名用于唯一标识每个微服务实例，并用于服务发现和路由

服务间通信：Feign不仅可以用于调用远程微服务，还可以用于本地服务调用其他微服务。即使这些服务在同一进程内运行，Feign仍然可以用来发起HTTP请求，提供了一种统一的请求方式。

### 常见微服务架构：

组件:

SpringCloud：全家桶+第三方插件（Netflix）**闭源**了，不用了

SpringCloud Alibaba：（主流）

注册中心：nacos

配置中心：nacos-config

断路器：sentinel

SpringCloudAlibaba致力于提供微服务开发的一站式解决方案。

## 两个项目互相调用实现：

1. rest接口使用依赖springMVC===》添加spring-boot-start-web依赖
    
2. 创建controller，再此类上加上@RestController
    
3. @SpringBoorApplication创建启动类（是一个@Repository配置类)（配置ResTemplate到配置类中）
    

> @Bean public RestTemplate restTemplate(RestTemplateBuilder builder){ RestTemplate restTemplate = builder.build(); return restTemplate;
> 
> }

4. 在调用类中注入restTemplate,在体哦用方法中使用restTemplate.getForObject(url,返回值类型),并用
    

建议打开run dashboard,启动图标->editconfiguration->template->"+"->"springboot"

### @RestController注解作用：

**标识控制器类**： `@RestController` 告诉 Spring，该类是一个控制器（Controller），用于处理 HTTP 请求。

**自动序列化返回值为 JSON 或 XML**：

- 通过默认的 `MessageConverter`（如 Jackson），将方法返回的对象序列化为 JSON（或其他支持的格式，如 XML）。
    
- 适合开发 API 服务。
    

**简化配置**：

- 不需要单独使用 `@ResponseBody`，每个控制器方法的返回值都会自动作为响应体返回。
    

## 配置nacos

上述方法必须通过url访问其他服务,服务器地址变化(迁移等) 、服务下架不可访问、水平扩展、因此要手工改代码，很繁琐，于是产生了注册中心（对应SOA治理中心），处理容错和ip问题等。

### springboot和Springcloud和Springcloudalibaba的关系：

**Spring Boot** 是基础，它为Spring Cloud 和 Spring Cloud Alibaba 提供了基本的运行环境和自动配置支持。

**Spring Cloud** 是构建微服务的框架，它在Spring Boot的基础上添加了微服务的相关功能。

**Spring Cloud Alibaba** 是对Spring Cloud的扩展，结合了阿里巴巴的分布式微服务技术。

因此，它们之间的关系是包含和扩展的关系：Spring Cloud Alibaba包含Spring Cloud，并在其基础上进一步扩展了阿里巴巴的微服务技术。

### idea创建项目选项解释：

> 这张图片展示了在IntelliJ IDEA中创建新项目时的不同选项分类，具体包括以下几部分的信息：
> 
> \1. Java
> 
> - Java：这是最基本的选项，用于创建一个纯Java项目。
> 
> \2. Java Enterprise
> 
> - Java Enterprise：用于创建企业级Java项目，如J2EE、Java EE等。
> 
> \3. JBoss
> 
> - JBoss：为使用JBoss应用服务器的Java项目提供支持。
> 
> \4. J2ME
> 
> - J2ME：创建移动设备上的Java应用。
> 
> \5. Clouds
> 
> - Clouds：用于创建与云计算相关的项目，例如Amazon Web Services (AWS)、Google Cloud Platform (GCP)等。
> 
> \6. Spring
> 
> - Spring：创建基于Spring框架的项目，包括Spring MVC、Spring Data等。
> 
> \7. Java FX
> 
> - Java FX：用于创建基于JavaFX的桌面应用。
> 
> \8. Android
> 
> - Android：创建Android应用项目。
> 
> \9. IntelliJ Platform Plugin
> 
> - IntelliJ Platform Plugin：用于创建IntelliJ IDEA插件。
> 
> \10. Spring Initializr
> 
> - Spring Initializr：如前文所述，快速创建Spring Boot项目的工具。
> 
> \11. Maven & Gradle
> 
> - Maven：选择Maven进行构建。
> 
> - Gradle：选择Gradle进行构建。
> 
> \12. Groovy
> 
> - Groovy：创建基于Groovy语言的项目。
> 
> \13. Grails
> 
> - Grails：用于创建Grails框架项目。
> 
> \14. Application Forge
> 
> - Application Forge：创建基于Forge框架的项目。
> 
> \15. Static Web
> 
> - Static Web：用于创建静态网页项目。
> 
> \16. Flash
> 
> - Flash：创建Adobe Flash项目。
> 
> \17. Kotlin
> 
> - Kotlin：创建基于Kotlin语言的项目。
> 
> \18. Empty Project
> 
> - Empty Project：创建一个空白项目，可以从头开始设置和开发。

### Nacos功能

- _服务发现_ 和 _服务健康监测_
    
- _动态配置服务_
    
- _动态DNS服务_
    
- _服务及其元数据管理_
    

### 搭建实现步骤：(在springboot项目里)

#### 搭建SpringCloudAlibaba

1. 父pom文件添加Alibaba依赖（由版本适配问题，官网或github查看版本说明）
    
2. 添加<dependencyManagement> <dependencies> <dependency> <groupId>com.alibaba.cloud</groupId> <artifactId>spring-cloud-alibaba-dependencies</artifactId> <version>2.2.5.RELEASE</version> <type>pom</type> <scope>import</scope> </dependency> </dependencies> </dependencyManagement>（dependencyManagement里面可以继承多个父项目）
    
    > 另一个方法创建和导入：脚手架
    > 
    > 创建springboot时使用initializr service url为：http: //start.aliyun.com/
    

#### 搭建注册中心：

就是一个nacos服务器，他有四个接口，即注册接口、服务获取接口、心跳接口、注销接口

1. 下载解压（与alibaba版本匹配）nacosgithub.com/alibaba/nacos/releases
    
2. 双击（默认集群模式）
    

startup.cmd编辑mode=standalone

> nacos/conf/application.conf的配置：

> 虚拟目录、端口（8848）、连接超时、最大连接数、数据源（默认在内存）、持久化

成功界面：

![image-20241219214717721](file:///C:/Users/chen2/AppData/Roaming/Typora/typora-user-images/image-20241219214717721.png?lastModify=1757213953)

3. 在浏览器访问所给的地址，打开界面
    
4. 在微服务中添加配置：
    
    1. 所有需要注册的微服务的pom.xml添加依赖
        
    
    > <！--nacos-服务注册发现- <dependency> <groupId>com.alibaba.cloud</groupId> <artifactId>spring-cloud-starter-alibaba-nacos-discovery</artifactId> </dependency>
    
    2. 配置文件：（名称）
        
    
    ![image-20241219220031812](file:///C:/Users/chen2/AppData/Roaming/Typora/typora-user-images/image-20241219220031812.png?lastModify=1757213953)
    
    3. 启动微服务，发现注册到对应命名空间中了
        

> 微服务==nacos客户端（分为消费者（调用）和提供者（被调））