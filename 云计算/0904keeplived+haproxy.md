---
date: "20250904"
docx:
  - 2-keepalived.docx
  - haproxy.docx
---
[keepalived.docx](附件/集群架构/2-keepalived.docx)
keepalived是lvs组件，lvs常结合keepalived使用
[harproxy.docx](附件/集群架构/3-haproxy.docx)

>- HAProxy **可以单独做 L7 负载均衡**
>- 如果想要高可用 VIP，需要 **结合 Keepalived**
>- 如果同时需要 **海量 TCP 转发 + HTTP 应用层控制**，就用 **LVS + Keepalived + HAProxy** 配合


>- 负载均衡集群中，只有接管 VIP 的节点处理流量，其它备用节点闲置，只有在故障转移时才激活
# keepalived

## 笔记
==Keepalived介绍==

==keepalived文件介绍==

重点配置：
![[Pasted image 20251010102909.png]]
## ==keepalived配置实验==

![[Pasted image 20251013193451.png]]

### ==配置keepalived抢占模式==
默认
主要配置
![[Pasted image 20251013180233.png]]
![[833dad747c8cfedc1713a0a5141ef54.jpg]]
![[806b9552c544ef080889d4e1b070653.jpg]]
![[7d06f2e8e40db4b6eb4d532f323ae3e.jpg]]
### ==配置keepalived非抢占工作模式==

![[Pasted image 20251013185916.png]]

![[e7468a046cc5af4c9af0025d609f1f8.jpg]]
![[108ba70a5577c1c1a90f9c05d00f288.jpg]]
### nginx访问不了vip解决：
删除或注释此项
![[Pasted image 20251013190006.png]]
![[8703cc1f940f547b72b7865456d9f68.jpg]]


# haproxy
## 笔记
==Haproxy介绍==

==Haproxy代理模式==

==Haproxy调度算法==

==Haproxy与nginx和lvs对比==

==Haproxy主配置文件介绍==

## 实验

![[Pasted image 20251013193523.png]]

==搭建部署==
![[Pasted image 20251013195116.png]]

==会话保持==
![[Pasted image 20251013195213.png]]
==后端健康状态检查==
![[Pasted image 20251013200402.png]]
## 补充

 ==四层、七层lb解析流程==

>四层代理（L4）
>	只解析 TCP/UDP 包的 **源 IP、目标 IP、端口号**
   七层代理（L7）
   >	解析tcp，同时解析应用层协议（HTTP、HTTPS 等）
   

 四层（L4）负载均衡流程

1. **解析 TCP 层**：获取源 IP、目标 IP、源端口、目标端口
    
2. **选择目标后端服务器群**
    
3. **根据负载均衡算法选择具体服务器**：轮询、最少连接、源 IP 哈希等
    
4. **转发 TCP 连接**：把 TCP 流量发送到选中的服务器
    

---

七层（L7）负载均衡流程

1. **解析 TCP 层**：建立连接
    
2. **解析应用层内容**：HTTP URL、Host、Header、Cookie 等
    
3. **筛选目标服务器群**：根据应用层 ACL 或规则
    
4. **根据调度算法选择具体服务器**：轮询、最少连接、会话粘性等
    
5. **转发请求**：把请求发送到选中的服务器
    

---

💡 **总结**：

- 不论 L4 还是 L7，都先确定“服务器群”（虚拟服务对应的后端集群）
    
- 然后在群里根据算法选择具体服务器
    
- L7 在选择群时会用到应用层内容，L4 只用 TCP 信息


**两个概念**：
虚拟服务 = 前端入口 (frontend)
后端集群 = backend server{} 内的服务器组

==nginx、LVS、Haproxy对比==

功能对比：

|功能|LVS|Keepalived|HAProxy|Nginx|
|---|---|---|---|---|
|L4（TCP/UDP）负载均衡|✅|依赖 LVS|✅|✅（可做 TCP/UDP，但性能比 LVS/HAProxy低）|
|L7（HTTP/HTTPS）负载均衡|❌|❌|✅|✅|
|高可用（VIP 漂移）|❌|✅|❌|❌（可通过 Keepalived 实现 VIP）|
|健康检查|❌|✅|✅|✅|
|静态文件处理（动静分离）|❌|❌|❌|✅|
|SSL/TLS 终端处理|❌|❌|✅|✅|

>健康检查 = 后端可用性管理，故障转移 = 入口可用性保障（VIP 漂移）
>



==反向代理、负载均衡、web服务==

- **Web 服务**：Nginx 自己直接返回本地静态资源（`root` / `index`）
    
- **反向代理**：Nginx 把请求转发给单个后端服务（`proxy_pass` 指定地址）
    
- **负载均衡**：Nginx 把请求转发给多个后端服务（`proxy_pass` 指向 `upstream` 集群 + 调度算法）


>- **负载均衡 ⊂ 反向代理**
   > 
>- 所以可以总结为：
>
  >  1. **反向代理不一定是负载均衡**（因为可能只有一个后端）
    >    
    2. **负载均衡一定是反向代理**（因为必然涉及代理 + 多后端）


 ==七层和四层关系梳理==

1. **反向代理**
    
    - 一定是 **七层（应用层）** 的。
        
    - 功能：代理请求，把客户端请求转发到后端。
        
2. **负载均衡**
    
    - 分两类：
        
        - **四层负载均衡（L4）** → LVS、HAProxy(L4)
            
            - 基于 IP+端口分发，不解析应用层协议。
                
        - **七层负载均衡（L7）** → Nginx、Apache、HAProxy(L7)
            
            - 基于 URL、Header、Cookie 等应用层内容分发。
                
            - 本质上是 **反向代理 + 调度算法**。
                
3. **两者关系**
    
    - 反向代理 **必定是七层的**。
        
    - 七层负载均衡 = 反向代理 的扩展（在代理基础上加上后端集群+调度）。
        
    - 四层负载均衡不涉及反向代理。
        

---

✅ 所以一句话总结：

- **反向代理 = 七层代理**
    
- **负载均衡 = 四层 or 七层**
    
- **七层负载均衡 = 反向代理 + 调度算法**

 
