---
date: "1010"
docx:
  - 2-KVM+NFS实现动态迁移.docx
  - 镜像-容器.docx
实验: 未做
---
[2-KVM+NFS实现动态迁移.docx](附件/虚拟化/2-KVM+NFS实现动态迁移.docx)
[镜像-容器.docx](附件/虚拟化/镜像-容器.docx)
# KVM+NFS实现动态迁移
==介绍==

==实验==

![[Pasted image 20251028135518.png]]

```
虚拟系统管理器迁移：

虚拟机磁盘放在共享 NFS
打开 virt-manager虚拟系统管理器，右键虚拟机 → 迁移
选择目标主机 → 点击确认
virt-manager 调用 libvirt/QEMU 完成 内存 + CPU 状态迁移
虚拟机在目标主机继续运行，用户几乎感知不到停机
```

# docker
## 笔记

==Docker安装==

==Docker镜像操作==

==Docker容器操作==

==容器端口的映射==

==Docker数据管理==

==容器的导出与导入==

==容器的删除==

==文件复制==


==Docker资源限制==

## 补充

>镜像（Image）是模板 → 容器（Container）是运行实例 → 仓库（Repository）是镜像存放处。