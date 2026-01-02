---
date: "20250911"
docx:
  - redis哨兵.docx
  - saltstack.docx
实验: 已做
---
[redis哨兵.docx](附件/缓存/5-redis哨兵.docx)
[saltstack.docx](附件/平台管理/saltstack.docx)
# redis哨兵
## 概念
![[Pasted image 20250911083637.png]]
总结：redis主从复制从节点的作用；普通主从复制需要手动故障恢复
![[Pasted image 20250911084056.png]]
总结：sentinel实现HA+其工作流程
架构名称：Redis Sentinel 架构
## 实验
一主两从3哨兵
实验目的：部署redis sentinel1主两从3哨兵结构
机器：3台vm
效果：主机挂掉，slave自动故障恢复
![[f2c88d6d5c6d924a19ea8258544fcf09.jpg]]
![[197099a01d1e0351e59ce0eb06c7d582.jpg]]
## 补充
==推荐是sentinel和master/slave同主机吗？==

- **大规模/高可用要求极高** → **完全分开**
    
- **中小规模生产** → **部分同机**（常见）
    
- **学习/测试** → **全部同机**


==主从复制+sentinel一般是多master还是单master？==
 **单 master + Sentinel** 更常用，而 **多 master + Sentinel**很少用
 单master：
 - 适合中小规模业务
    - 数据量和并发不大，分片不必要
	简洁、高可用、易管理
多 master + Sentinel：
	**管理成本高，容易出错**

==多master+sentinel架构与cluster主从复制架构的区别？==

|架构|数据管理|分片|高可用|扩展性|
|---|---|---|---|---|
|多 master + Sentinel|独立库|❌ 手动|✅ 单 master 内|差，需要手动|
|Redis Cluster|Hash slot 分片|✅ 自动|✅ 自动|好，节点可动态加入|

简单理解：

- **多 master + Sentinel = 多个独立高可用数据库**
    
- **Redis Cluster = 自动分片 + 高可用集群**

==redis各架构适用情况？==
- 如果数据量小、单库可用 → 单 master + Sentinel
	- 高可用简单版
- 如果需要分片、高并发 → Redis Cluster（自带多 master + 自动故障转移）
	- 分片 + 高可用完整版

实际生产环境中，几乎很少使用 **多 master + Sentinel** 架构：管理复杂、扩展性差

==sentinel投票机制与cluster机制的区别？==
Sentinel 架构
- **候选者**：宕机 master 的 slave
- **参与投票**：Sentinel 节点（监控 master）
- **流程**：
    1. Sentinel 检测 master 宕机（客观下线 ODOWN）
    2. Sentinel 节点互相投票，选出一个 slave 升级为 master
    3. 通知其他 slave 和客户端
Redis Cluster 架构
- **候选者**：宕机 master 的 slave
- **参与投票**：宕机 master 的 slave（slave 之间互投票）
- **流程**：
    1. 集群中多数 master 判断某 master 宕机（fail）
    2. 宕机 master 的 slave 互相投票，选出新的 master
    3. 新 master 接管 slot，集群更新映射
==投票对象必须是奇数吗？==
- **Sentinel 架构**：要求 Sentinel 节点数量为奇数，以避免平票，保证选举可靠。
- **Redis Cluster 架构**：不强制要求 slave 数量为奇数，因为选举不仅依赖宕机 master 的 slave 投票，还结合多数 master 的判断，平票风险低，所以可以是偶数或奇数
==密码一致问题？==
- **主从复制**：只要求 `Slave.masterauth = Master.requirepass`。
- **主从 + Sentinel**：要求 **Master.requirepass = Slave.requirepass = Sentinel.auth-pass**，保持完全一致。
==sentinel集群和reids主从集群的关系？==
✅ **Redis 哨兵集群（Sentinel）不是 Redis 集群的一部分**，  
而是一个 **独立的高可用监控与自动故障转移机制**。
# Saltstack
## 概念
![[Pasted image 20250911163939.png]]
## 实验
目的：搭建saltstack服务，管理多台客户端
机器：3台主机
	一台master+minion
	两外两台minion
效果：远程控制管理

![[032a5fa9b92b62f65ab990f19ea31ebf.jpg]]
![[8036d6f913f609f1bd4accba39b69616.jpg]]
## 补充
==三个rpm包的作用是什么？==
salt-2.el7.noarch.rpm  
	Salt 的基础包（公共依赖），不单独运行服务，但为 master 和 minion 提供共同依赖
salt-master-2.el7.noarch.rpm  
	Salt Master 端服务包
	安装后提供 `salt-master` 服务
    管理所有的 minion 节点
    接收并下发命令、管理状态、执行任务 
	在控制端（比如运维管理机）安装。
salt-minion-2.el7.noarch.rpm
	Salt Minion 端服务包
	安装后提供 `salt-minion` 服务
    接收 master 下发的命令
	在本机执行（如装包、改配置、执行脚本）并返回结果
	在被管理的业务服务器上安装。

- `salt` → 公共依赖
- `salt-master` → 控制端
- `salt-minion` → 被控端
==zeromq-devel是什么？==
是一个开发包，包括头文件+链接库；
-devel
	development表示开发包；

==rpm -ivh和yum localinstall的区别？==
- `rpm -ivh xxx.rpm` 只安装包本身，不会解决依赖问题。
- `yum localinstall xxx.rpm` 会用 yum 仓库去解决依赖，适合网络可用的情况。
==配置文件和sls脚本文件都是yaml格式，有哪写格式上的注意？==

：与后面的值之间有空格
缩进只能用空格
-与键之间有空格
