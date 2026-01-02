---
date: "20250820"
docx:
  - dns.docx
  - dhcp.docx
  - rsync.docx
实验: 已做
---
dns.docx
dhcp.docx
rsync.docx
# DNS
## 笔记

==DNS概念==

>ip直接通讯--hosts文件--DNS分布式数据库
域名解析  域名--ip  正向、反向
**端口**  tcp和udp53  FQDN  DNS查询模式  迭代  递归
DNS分布式结构

    
 ==DNS请求流程==
 
>浏览器缓存--操作系统hosts文件--本地dns缓存--递归+迭代查询--权威服务器（从根域往下）--

==DNS搭建==

## 实验
主要配置部分：
/etc/named.conf
![[Pasted image 20251010111408.png]]
/var/named/test.zheng
![[Pasted image 20251010111549.png]]
/var/named/test.fan
![[Pasted image 20251010111621.png]]



实验目的：搭建dns服务器（包管理器）
机器：两台vm
效果： DNS 服务器接收客户端查询请求，解析域名和 IP 地址
局限：日志记录未配置
![[94676bd34704b045845491e6b629b51.jpg]]

![[67acf2e5aee0546ffa08efb6ec23178.jpg]]

![[00d2308aeaf09367c4cf71528041e08.jpg]]

流程总结:

> **下载**bind和工具
> **配置**主配置文件：定义全局、正向反向区域
> **创建**区域文件+设置**权限**
> **启动**bind
> **测试**

## 补充

==域名层级==
>国际标准域名体系 (DNS) 里，只有：
>- 根域（Root, `.`）
>- 顶级域名（Top Level Domain, TLD，比如 `.com`、`.org`、`.cn`）
>- 二级域名（Second Level Domain，比如 `example.com`）
>- 三级域名、四级域名……（比如 `www.example.com`、`blog.dev.example.com`）
>- 严格意义上没有“一级域名”这个说法。
但是在中文语境下，经常有人把 **`example.com` 叫做一级域名**，其实这就是国际上标准说法里的“二级域名”。

==bind运行用户是谁？==

>  `named` 守护进程会以 `named` 用户身份运行，而不是 `root`，要创建
>  
>- 包管理器安装 → 系统帮你建好 `named` 用户了。
>- 源码安装 → 需要你自己手动创建 `named` 用户，再设置配置目录和 zone 文件权限。
>- 
>-权限配置原则： 
>- 不用 root 用户运行服务，避免安全风险
>- 运行用户必须对配置文件、zone 文件和日志目录有读写权限

==域名记录类型有哪些？==
>常见的地址解析记录:
NS记录/主域名记录：记录当前区域的DNS服务器的主机地址。
MX记录/邮件记录：记录当前区域的邮件服务器的主机地址，数字10表示（当有多个MX记录时）选择邮件服务器的优先级，数字越大优先级越低。
A记录/正向域名记录：记录正向解析条目（IPV4）。
AAAA记录/正向ipv6域名记录：记录正向解析条目（IPV6）。
CNAME记录/别名记录：记录某一个正向解析条目的其他名称。
cat /var/named/named.ca   \#全球13台根服务器

==bind、bind-utils的作用是什么？==

|软件包|功能|是否提供 DNS 服务|
|---|---|---|
|bind / bind9|DNS 服务器守护进程（named）|✅|
|bind-utils / dnsutils|客户端测试工具、管理工具|❌|



# DHCP

## 笔记

==DHCP介绍==

>udp67  68
给客户端分配ip、网关、掩码、dns
dhcp地址类型  动态分类（有时限）  静态分类
租约过程

==安装DHCP服务==
## 实验

主要配置
dhcpd.conf
![[Pasted image 20251010112026.png]]
![[Pasted image 20251010112117.png]]
server修改为静态ip
![[Pasted image 20251010112358.png]]
![[Pasted image 20251010112548.png]]

静态ip配置
![[Pasted image 20251010113633.png]]

---

### 动态分配实验

![[42a5843dc8816c252b01a3a3968286f2.jpg]]
测试
![[96809bcd1f6315ae692c16e9ee05adbb.jpg]]
```
ip a
systemctl restart network   #获取方法之一
dhclient -f ens33
dhclient -d ens33           #获取方法之二
```

### 静态分配实验
![[72c72ed823934f98b0d71b81581e8089.jpg]]

# rsync
## 笔记

==rsync简介==

>异地文件备份工具 


==rsync服务角色==

>客户机---下行上传同步---服务器
rsync使用
-a     //递归复制目录 + 保留权限
-v     //可视化
-z     //压缩
文件：
	![[Pasted image 20250914215308.png]]
目录：
	![[Pasted image 20250914215244.png]]
>
scp使用
scp -rcp
-r      //目录递归
-C    //压缩
-p    //保留原属性
文件：
	![[Pasted image 20250914214955.png]]
目录：
	![[Pasted image 20250914215055.png]]
>
`scp` 和 `rsync` 都是基于 SSH 的安全文件传输工具，  
配置密钥互信后就可以免密码登录，  
其中 `rsync` 更强大，支持增量、断点续传和同步功能。

==配置rsync备份源==
## 实验

Client/Server（客户端/服务端）模式搭建：
rsync --daemon 服务进程，监听端口（默认 873），等待客户端连接。
Client（rsync 命令）：主动连接服务端的 rsyncd，根据模块名同步数据

Rsync 的 C/S 架构通过守护进程 rsyncd 在服务端监听 873 端口。  
服务端配置 `/etc/rsyncd.conf` 定义同步模块和用户密码文件，启动 `rsync --daemon`。  
客户端使用 `rsync user@server::module` 语法进行同步。  
通常结合 `--password-file` 实现免交互同步，常用于内网自动化备份

关键配置
主配置文件：/etc/rsyncd.conf
![[Pasted image 20251010114017.png]]
/etc/rsyncd_pass密码文件600
![[Pasted image 20251010114119.png]]
源目录：
![[Pasted image 20251010114201.png]]


目的：rsync备份源服务搭建实验
机器：两台vm
效果：客户端可以拉取和上传文件目录
![[a612fa32b1512eba38f1f2d073fece9a.jpg]]
```
```

流程总结：
>**修改**/etc/syncdconf 配置文件
>**创建**账户文件，授权
>**创建**模块目录，授权
>**启动**服务
>**测试**


无交互验证方式实验+定时拉取实验
![[be83bc4ec4edb0c80bb889855ddbceb2.jpg]]
```
自动化备份
crontab -e
59 23 * * * /usr/bin/ rsync -avz  --delete --password-file=/etc/rsyncd_pass backuper@192.168.80.45::wwwroot /opt/

```
## 补充
==不建议用root运行rsync服务==？
> 在 `rsyncd.conf` 中不建议使用 `uid=root` / `gid=root`，  
> 因为这会让同步进程获得系统最高权限，带来严重安全风险。

✅ **推荐做法：**

- 创建专用用户（如 `rsync`）；
    
- 限制访问目录；
    
- 配合防火墙和认证文件保护服务。


==文件目录权限==

服务端  账户密码file：
专用用户 600。
chown rsync: rsync /etc/rsyncd.secrets
chmod 600 /etc/rsyncd.secrets

服务端 模块所在目录：
属主：配置的 ==uid/gid 用户(为实际映射用户)==。
权限：保证该用户有 r 或 rw 权限。
chown -R rsync:rsync /data/backup
chmod 750 /data/backup

客户端密码文件：
属主：执行 rsync 的用户。
权限：600。
chmod 600 /etc/rsync.pass

这样就能确保 安全性 + 正常工作