---
date: "20250908"
docx:
  - mongodb部署.docx
实验: 已做
---
[mongodb部署.docx](附件/mongodb部署.docx)
# mongodb
## 笔记

==mongodb简介==

==应用场景==

==mysql和mongodb对比==

==mongodb常见的数据类型==

==安装mongodb前系统配置==

==二进制安装mongodb==

==rpm安装mongodb数据库==

==mongodb操作==

==数据导入导出及备份==

==mongodb副本集==


## 实验
![[Pasted image 20251013215038.png]]


>老版本自带客户端mongo，新版本客户端要另行安装mongosh


==同副本集认定是什么==？

|判断项|是否必须相同|说明|
|---|---|---|
|**`replSetName`**|✅ 必须一致|唯一标识副本集名称，不同则不能加入同一个集群|
|**副本集配置中的成员列表（host:port）**|✅ 必须包含该节点|否则不会被识别为合法成员|
|**认证密钥（keyFile）**|✅ 若启用安全认证|否则节点无法互信通信|
|**网络可达性**|✅ 必须能相互通信（TCP）|不一定要同网段，但要能互通|
