---
date: "20250831"
docx:
  - loacation-rewrite-动静分离.docx
  - tomcat安装.docx
实验: 已做
---
  
# 回顾
![[2a7232ae5027783d211d02d851e4e34.jpg]]

# loacation-rewrite-动静分离 
## 笔记

 ==nginx location匹配规则==

>![[2b34cfff499f5823beeb1098aefa1f3.jpg]]
>
>![[f0c9cfc8087ee8b445be44fd2c9ca00.jpg]]


 ==nginx rewrite匹配规则==

>就是不断修改url，当前url匹配后替换成新url
>
>![[76f8ede3f9a7a8d8d546c9ba4df6921.jpg]]
>
>![[286fbb5b25f895601d59d639dbfed03.jpg]]

| 标志            | 类型   | 是否重新匹配 `location` | 是否返回给客户端           | 常见用途                           | 说明                            |
| ------------- | ---- | ----------------- | ------------------ | ------------------------------ | ----------------------------- |
| **last**      | 内部跳转 | ✅ 是               | ❌ 否                | 内部重写到另一个 location（如 index.php） | 改变 URI 后重新走一遍 location 匹配流程   |
| **break**     | 内部跳转 | ❌ 否               | ❌ 否                | 在当前 location 内部改 URI 并立即执行后续指令 | 不再执行后续 rewrite，但继续执行当前块内的其他指令 |
| **redirect**  | 外部跳转 | ❌ 否               | ✅ 返回 **302 临时重定向** | 临时跳转到新 URL                     | 浏览器地址栏改变，不缓存                  |
| **permanent** | 外部跳转 | ❌ 否               | ✅ 返回 **301 永久重定向** | 永久跳转（SEO 优化）                   | 浏览器会缓存跳转结果，下次不再请求旧 URL        |

==nginx内置变量==

>![[2f835ff181201e4029329d1f7ea60dc.jpg]]
    [[杂碎知识#nginx变量来源：|nginx变量来源]]

==nginx动静分离==


## 实验
### location优先级实验
没注释= /
测试：

![[Pasted image 20251006210146.png]]
注释=/
![[Pasted image 20251006210121.png]]
测试：
![[Pasted image 20251006210308.png]]

![[Pasted image 20251006210444.png]]
### rewrite实验

>略
>last|break、redirect、permanent
### 动静分离实验

location里的 ==proxy_pass==指定动态页面转发
配置的是反向代理，**不是 FastCGI**
原理：
![[Pasted image 20251010164713.png]]

![[Pasted image 20251010164922.png]]
 ![[Pasted image 20251010164948.png]]
 ![[b986d85c90240646c03f0d27ce7086b2.jpg]]
实质就是同一个server里的**不同location**，一个location处理静态资源（使用root和index字段），另一个location使用proxy_pass指定新的server服务器ip和端口
php页面存在

总结：
```
php服务器：

yum php httpd

start httpd

PHP页面根目录路径:/var/www/html/

下创建index.php

![image-20251008191504496](file:///C:/Users/chen2/AppData/Roaming/Typora/typora-user-images/image-20251008191504496.png?lastModify=1759923998)

访问ip:80看到php页面=》这里使用的是传统php模块，而不是PHP-FPM #**只要使用 mod_php，Apache和PHP就注定要在一起！**

nginx服务器：

nginx.conf

+location ~ \.php {proxy_pass [http://192.168.150.10](http://192.168.150.10/) }

#这里是反向代理到了192.168.150.10:80/index.php,也就是代理到了httpd服务上
```
## 补充
==http请求字段？==

请求行格式：
```
METHOD Request-URI HTTP-Version
```
HTTP 请求行确实只有且必须有这 3 个字段：
1 请求方法
	 - Nginx变量: `$request_method`
2 请求URI
	Nginx变量:$request_uri
		- 组成部分:
	    - 路径: `/api/v1/users` 
	    - 查询字符串: `?id=123&name=john`
3 HTTP版本
	nginx变量：`$server_protocol`

请求头：
	Host是必须存在的；其余字段可选；
	其余字段：

| 类别        | 字段                                             | 说明                             |
| --------- | ---------------------------------------------- | ------------------------------ |
| **必须**    | **`Host`**                                     | HTTP/1.1 强制要求，用于虚拟主机。          |
| **有条件必须** | **`Content-Length`** 或 **`Transfer-Encoding`** | 当请求带有 Body 时，二者必须其一，用以界定 Body。 |
| **事实标准**  | `User-Agent`, `Accept`, `Connection`           | 虽然不是协议强制，但现代浏览器几乎总会发送。         |
| **真正可选**  | `Cookie`, `Referer`, `Authorization` 等         | 仅在特定场景下需要时才出现。                 |

==http字段对应nginx变量？==
请求行：

| 请求行部分            | 字段说明          | Nginx 变量名                        |     |
| ---------------- | ------------- | -------------------------------- | --- |
| **METHOD**       | 请求方法          | **`$request_method`**            |     |
| **Request-URI**  | 完整原始 URI      | **`$request_uri`**               |     |
|                  | 当前 URI (不含参数) | **`$uri`**, **`$document_uri`**  |     |
|                  | 查询字符串 (参数)    | **`$args`**, **`$query_string`** |     |
|                  | 是否有参数         | **`$is_args`** (有参数为 `?`, 否则为空)  |     |
| **HTTP-Version** | 协议版本          | **`$server_protocol`**           |     |

请求头：

| 请求头字段                 | 字段说明      | Nginx 变量名                                         |
| --------------------- | --------- | ------------------------------------------------- |
| **Host**              | 主机名       | **`$http_host`**, **`$host`**                     |
| **User-Agent**        | 客户端信息     | **`$http_user_agent`**                            |
| **Accept**            | 接受的内容类型   | **`$http_accept`**                                |
| **Accept-Encoding**   | 接受的压缩编码   | **`$http_accept_encoding`**                       |
| **Accept-Language**   | 接受的语言     | **`$http_accept_language`**                       |
| **Connection**        | 连接控制      | **`$http_connection`**                            |
| **Content-Length**    | 请求体长度     | **`$http_content_length`**, **`$content_length`** |
| **Content-Type**      | 请求体类型     | **`$http_content_type`**, **`$content_type`**     |
| **Cookie**            | Cookie 数据 | **`$http_cookie`**                                |
| **Referer**           | 来源页面      | **`$http_referer`**                               |
| **Authorization**     | 认证信息      | **`$http_authorization`**                         |
| **X-Forwarded-For**   | 客户端真实 IP  | **`$http_x_forwarded_for`**                       |
| **X-Real-IP**         | 客户端真实 IP  | **`$http_x_real_ip`**                             |
| **X-Forwarded-Proto** | 原始协议      | **`$http_x_forwarded_proto`**                     |


==nginx的url解析流程？==

请求 URI
  │
  ▼
匹配 location（精确 → 前缀 → 正则 → 普通）
  │
  ├─ 是文件？
  │     ├─ 是 → 静态或 PHP 执行
  │     └─ 否 → 目录处理
  │
目录请求
  │
  ├─ location 有 index 或继承 server index？
  │     ├─ 找到文件 → 内部重写 URI → 再次匹配 location → 执行
  │     └─ 没找到 → autoindex 决定返回目录列表或 403
  │
  └─ location + server 均无 index → autoindex 决定返回目录列表或 403


# Tomcat安装
## 笔记
 
==tomcat软件简介==

>是
	开源web应用服务器
原名 catalina
>
使用场景
>![[Pasted image 20250903165915.png]]
>![[Pasted image 20250903165824.png]]
>jdk介绍![[Pasted image 20250903170030.png]]
总结：
	**Tomcat = Web 服务器 + Servlet/JSP 容器**
	**Tomcat 需要 JDK 的根本原因：Tomcat 本身是用 Java 编写的，它需要 Java 运行时来执行**

==安装配置tomcat==（实验）

==tomcat目录介绍==

>总目录![[Pasted image 20250903171202.png]]
>
/lib:存放的都是jar包
/bin：可执行文件，就是脚本
/log：存日志文件
 >
**/webapps目录**
	![[Pasted image 20250903171220.png]]
Tomcat 默认会 自动解压部署 `webapps` 下的 `.war` 文件
`ROOT` 目录在 Tomcat 里确实是特殊的，它对应 Web 应用的根上下文 `/`
>
/**conf目录**
	![[Pasted image 20250903171040.png]]
/conf里最重要的是server.xml主配置文件
>
**server.xml主配置文件介绍**
	![[Pasted image 20250903171055.png]]
>	![[Pasted image 20250903211352.png]]


==server.xml参数说明==
>![[Pasted image 20250903211737.png]]
>
>![[Pasted image 20250903212303.png]]

==自定义默认网站目录==

==基于域名的虚拟主机==

## 实验
### 安装tomcat
jdk-8u371.rpm
tomcat-9.0.65.tar.gz二进制包
下载地址：[https://archive.apache.org/dist/tomcat/tomcat-9/](https://archive.apache.org/dist/tomcat/tomcat-9/)

不需要配置文件修改
![[Pasted image 20251011191409.png]]![[20c8b844c43b7944ca20bbf50eac913.jpg]]
![[Pasted image 20251011191719.png]]
效果是8080端口启动
浏览器可访问8080，出现：
![[Pasted image 20251008200441.png]]

### 修改默认网站目录

![[Pasted image 20251010170120.png]]
![[Pasted image 20251010170209.png]]

![[5404a6de2ff14d379521242b24196e2.jpg]]
### 搭建两个站点(基于域名的虚拟主机)
![[Pasted image 20251010170741.png]]
![[Pasted image 20251010170804.png]]
![[Pasted image 20251010170846.png]]
![[Pasted image 20251010170938.png]]
![[806d820cf24023f6bea11403c64e51f.jpg]]
![[1894d5f806aca58b965d5d0d939d1cc.jpg]]
## 补充

==tomcat对jdk版本要求==
![[Pasted image 20251012150216.png]]

==nginx和tomcat对比：==


| 特性     | Nginx                       | Tomcat                         |
| ------ | --------------------------- | ------------------------------ |
| 核心作用   | Web 服务器 / 反向代理              | Servlet 容器（Web 服务器）            |
| 支持协议   | HTTP/HTTPS、反向代理、负载均衡        | HTTP/HTTPS、Servlet/JSP 执行      |
| 虚拟主机配置 | `server {}` 块               | `<Host>` 标签                    |
| 静态文件处理 | 高效直接提供                      | 可以，但性能不如 Nginx                 |
| 动态请求处理 | 通过 FastCGI/代理（PHP、Python 等） | 内置 Servlet/JSP 解析（Java Web 应用） |


>- Nginx 常用于前端代理 + 静态资源处理
   > 
>- Tomcat 处理 Java Web 应用的动态请求


==什么是web应用服务器==？

| 特性     | Web 服务器              | Web 应用服务器                      |
| ------ | -------------------- | ------------------------------ |
| 处理请求类型 | 静态资源（HTML、CSS、JS、图片） | 动态内容（PHP、Java、Python 等）        |
| 性能优化   | 高吞吐量、低延迟             | 支持复杂业务逻辑，性能受应用影响               |
| 与数据库交互 | 很少                   | 常与数据库、缓存、消息队列等交互               |
| 示例     | Nginx、Apache         | Tomcat、WildFly、PHP-FPM、Node.js |
==server.xml具体结构？==

 ```
 Server
 ├─ Listener(s)        # 监听器，接收生命周期事件
 └─ Service(s)         # 一个 Service 包含 Connector + Engine
      ├─ Connector(s)  # IP + 端口，用于接收客户端请求
      └─ Engine        # 请求处理核心，引擎
           ├─ Host(s)  # 域名（虚拟主机）+ 应用根目录
            └─ Context(s) # Web 应用上下文
           ├─ Executor(s)  # 线程池执行器
 ``` 


详解

 `<Server>`
- port：Tomcat 停机监听端口（默认 8005）
- shutdown：关闭 Tomcat 的命令（默认 `SHUTDOWN`）
`<Listener>`
- 用于加载 启动监听器/环境初始化
 `<Service>`
- name：服务名称（Catalina 是默认值）
- 作用：把 Connector（连接器） 和 Engine（处理引擎） 组合在一起
 **`<Connector>`**
 **匹配ip和端口；转发对应的Engine**
- port：监听端口（HTTP 默认 8080，HTTPS 默认 8443）
- protocol：协议（HTTP/1.1、AJP/1.3 等）
- connectionTimeout：连接超时时间
- redirectPort：当请求需要 HTTPS 重定向时使用的端口
- address：指定ip，默认是所有ip，即0.0.0.0
 `<Engine>`
- name：引擎名称（Catalina 默认）
- defaultHost：默认虚拟主机名（匹配不到 Host 时使用）
 `<Host>`
- name：虚拟主机名
- appBase：Web 应用存放目录（默认为 `webapps`）
- unpackWARs：是否解压 WAR 包
- autoDeploy：是否自动部署新应用
- Connector 监听客户端请求（HTTP/HTTPS）

**connector、engine、host的关系？**
- **一个 Service 可以有多个 Connector**
    - 例如同时监听 **HTTP（8080）** 和 **HTTPS（8443）**
    - 或监听不同网卡 IP 的同一端口    
- **多个 Connector 都指向同一个 Engine**
    - 无论请求从哪一个 Connector 来（匹配到 IP + 端口），  
        都会交给 同一个 Engine 处理
- **Engine 负责分发到 Host**
    - Engine 根据请求的 Host header（或默认 Host）
    - 将请求路由到对应 `<Host>`，再到 `<Context>` → 应用
- **默认 Host**：如果 Engine 的 `defaultHost` 指向的 `<Host>` 没定义，Tomcat 会自动在 Engine 内创建一个默认 Host（name=localhost，appBase=webapps）
- 
**三层处理：**
connector匹配ip：端口（匹配port、address字段）
	**定位engine**
engine中的host匹配域名（匹配name字段，得到appBase）
	定位**应用根目录appbase**
context匹配path（匹配path，得到doBase）
	定位**应用目录docbase**
最后进一步拼接剩余uri，得到最终文件路径

然后找到对应的 Servlet 来处理请求：
	
> **URL 匹配是指浏览器请求的 URL（去掉上下文路径 `/context` 后的部分）与 web.xml 或注解中配置的 `<url-pattern>` 比较，决定请求由哪个 Servlet 处理。**
>
>- 只匹配 **虚拟路径**（context-path 之后的部分）
>- **物理文件路径**不参与匹配
>- 每个请求 最终只匹配到 **一个 Servlet**

**Context 目录下**，**动静态资源默认是不分离的**，没有额外配置的话，Tomcat **不会自动把静态资源和动态资源分开**
	
总结
>Context = 上下文
  在 Tomcat 中，每个 `<Context>` 对应一个 **Web 应用** 
-它的主要作用：
    1. **定义上下文路径（path）** → 浏览器访问 URL 的前缀
    2. **指定物理文件目录（docBase）** → 存放应用文件、JSP、静态资源
示例：
`<Context path="/mall" docBase="/usr/local/tomcat/webapps/mall" />
    -path = `/mall` → URL 的前缀
    -docBase = `/usr/local/tomcat/webapps/mall` → 文件实际存放位置

nginx+tomacat动静分离、单纯tomcat动静混合：

| 场景             | 静态资源        | 动态资源   | 分离情况     |
| -------------- | ----------- | ------ | -------- |
| 纯 Tomcat       | WAR 包内      | WAR 包内 | 默认混合，可配置 |
| Nginx + Tomcat | Nginx 目录/缓存 | WAR 包内 | 完全分离     |
