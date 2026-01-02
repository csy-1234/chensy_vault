---
date: "25250924"
docx:
  - 1-git使用.docx
  - 2-gitlab部署.docx
实验: 已做
---

[1-git使用.docx](附件/CI-CD流程/1-git使用.docx)
[2-gitlab部署.docx](附件/CI-CD流程/2-gitlab部署.docx)


![[Pasted image 20250924083644.png]]

# git使用

概述
版本控制系统
Git介绍
git 安装方式

>yum安装git(Centos自带git)
>编译安装:
>	依赖---解压配置编译安装---创建链接--启动git

初次运行Git前必配项
获取帮助
初始化及创建Git仓库
git客户端操作
# gitlab部署
Gitlab简介
Gitlab工作原理
Gitlab特性和优点
安装Gitlab

>rpm包方式：gitlab-ce-17.7.7-ce.0.el7.x86_64.rpm
>依赖-启动相关进程-安装localinstall-修配文-初始化命令（就启动了）
>浏览器访问ip：80
>root+密码：/var/log/gitlab/gitlab-rails/production.log

补充：
**内置 Git 仓库 + Web 前端 + CI/CD** 的一体化平台
所以 GitLab 不仅是 Git 仓库，还提供管理界面和 DevOps 工具。