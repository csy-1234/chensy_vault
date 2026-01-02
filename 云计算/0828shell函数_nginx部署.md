---
date: "20250828"
docx:
  - shell脚本函数.docx
  - web基础
  - nginx服务部署
实验: 已做
---
# shell脚本函数
## 笔记

==函数介绍==

>**是**
>    若干shell命令
	shell中的
	代码块

==管理函数==

>组成
		函数名
		函数体
>
定义
	写法一
	函数名() {
	    命令
	    return 返回值
	}
>
	 写法二
	function 函数名 {
	    命令
	    return 返回值
	}
	 写法三
	function 函数名 （） {
	    命令
	    return 返回值
	}
>
查看系统函数
declare -F
declare -f \[函数名\]//查看定义
unset 函数名
>
调用
当前命令行、脚本内部、单独函数脚本

==交互式环境定义函数==

>$> a(){
exho "a function is been called"
}
>$> a  \#即可执行

==脚本环境定义函数==

>先定义后使用

==函数文件==

>定义后可以在命令行载入或脚本载入“source 函数文件或点空格函数文件”
>
>在交互的命令行source 函数文件或脚本文件开头source 函数文件


==函数参数==

>就是位置变量
>#!/bin/bash
>
hello() {
    echo "第1个参数: $1"
    echo "第2个参数: $2"
    echo "参数个数: $#"
    echo "所有参数: $@"
}
># 调用函数并传参
   hello world linux

==函数变量==

>本地变量
   local a=111
   >优先级：普通、环境、本地/局部变量

![[Pasted image 20250907195346.png]]
# Web基础
## 笔记

==域名概述==

>域名解析--host文件--DNS

==网页基本术语==

>网页--html
网站--页面集合
主页--网站的主页 
域名--url的一部分，标识主机
HTTP--传输协议
URL--资源完整路径
HTML--程序语言
超链接--网页中元素，点击后跳转到指定url或触发动作，html里表现为a标签
发布上线--网站部署到生产环境

==HTML基本结构==

```
<!DOCTYPE html>
<html lang="zh-CN">

	<head>
		<meta charset="UTF-8">
		<title></title
	</head>
	
	<body>
	
	</body>
</html>
```

==web概述==

>万维网

==web版本==   

>1.0    2.0偏交互

==静态页面、动态页面==

>静态.html/.htm--不连数据库--不交互--不涉及高级语言--展示型网站
动态.php/.jsp--设计高级语言--涉及数据库--交互型网站

==HTTP==

>HTTP方法
	get请求资源
	head获取响应头
	post提交数据，创建资源
	put提交数据，完全更新资源
	patch提交数据，部分更新资源
	delete删除资源
	connect建立隧道
	options查询服务器支持的HTTP方法
	trace回显服务器收到的请求
HTTP状态码1-5
	1 信息响应 请求已接收，继续处理
	2 成功响应 请求已成功被接收并处理
	3 重定向响应 需要进一步操作以完成请求
	4 客户端错误 请求有问题（语法、权限、资源不存在）
	5 服务器错误 服务器处理请求时出错
	![[Pasted image 20251005154536.png]]
HTTP请求流程
	**DNS 解析**：把域名 `www.example.com` 转换为 IP 地址
	**建立 TCP 连接**：三次握手（HTTP 基于 TCP） **浏览器发送 HTTP 请求报文**
	**服务器接收并处理请求**
	**服务器返回 HTTP 响应报文**
	**浏览器解析响应内容并渲染页面**
	**关闭 TCP 连接（或复用 Keep-Alive）**
>	
HTTP请求报文格式
	请求行头体
HTTP响应报文格式
	响应行头体



# nginx服务部署
### 笔记

==web服务器介绍==

>web服务器
	nginx  轻量级 高并发
	apache  并发能力低于nginx  稳定性
	openresty
	tengine
	Tomcat
	IIS
nginx与apache区别
	![[Pasted image 20251014154630.png]]

 ==IO模型==
 
 >**描述了**
	 应用程序与操作系统内核之间，进行数据读写时的交互方式。
	 当一个程序要从网络或磁盘读取数据时，进程是怎么等待、怎么获取数据的
一次 I/O 操作其实包含两个阶段
	等待数据准备阶段
		据还没到达网卡缓存区
		磁盘还没把数据读到内核缓存
	数据拷贝阶段
		从 内核空间 → 用户空间 拷贝数据	
同步、异步
阻塞、非阻塞
IO多路复用
信号驱动式IO
异步IO
图记：
	阻塞IO：     |<------- 用户等待（阻塞） --------->|
	非阻塞IO：   |轮询|轮询|轮询| 拷贝数据 |
	多路复用：   |select阻塞| 拷贝数据 |
	异步IO：     |---- 内核异步完成 ----| 通知用户 |

==nginx支持的并发模型==

>    select
	poll
	**epoll**
	kqueue
	/dev/poll

==工作原理==

>master进程
work进程--多个连接

==编译安装nginx==

## 实验
  都使用==源码方式==安装
  1.28.0
  软件包下载目录：[https://nginx.org/en/download.html](https://nginx.org/en/download.html)
直接安装就行不需要配置文件修改
安装目录/usr/local/nginx

![[5e455f4eb0516e43de63d1cfeed0eba.jpg]]

![[df0ab55ad87613f2836eab9499f7813.jpg]]
效果是80端口启动

```

1.yum -y install gcc pcre-devel zlib-devel openssl-devel elinks geoip-devel php-gd gd-devel

pcre-devel#正则表达式解析
zlib-devel#gzip 压缩功能
openssl-devel#HTTPS / TLS 支持
elinks#测试网页
geoip-devel#根据 IP 地址进行地理位置识别
php-gd 和 gd-devel#图像处理库

  ./configure \
--prefix=/usr/local/nginx \
--user=nginx \
--group=nginx \
--with-http_stub_status_module \
--with-http_ssl_module \
--with-http_flv_module \
--with-http_gzip_static_module \
--with-http_v2_module \
--with-http_realip_module \
--with-http_addition_module \
--with-http_image_filter_module \
--with-http_geoip_module \
--with-http_gunzip_module \
--with-pcre \
--with-stream \
--with-stream_realip_module

--prefix	安装路径
--user	--group	指定运行用户/组
--with-http_stub_status_module	状态监控模块
--with-http_ssl_module	支持 HTTPS
--with-http_flv_module	支持 FLV 视频流
--with-http_gzip_static_module	持预压缩的 gzip 文件
--with-http_v2_module	支持 HTTP/2
--with-http_realip_module	获取真实客户端 IP
--with-http_addition_module	响应内容追加
--with-http_image_filter_module	图像处理模块
--with-http_geoip_module	根据 IP 地址定位
--with-http_gunzip_module	支持 gunzip 解压
--with-pcre	启用正则表达式支持
--with-stream	支持四层代理（TCP/UDP 反向代理）
--with-stream_realip_module	stream 获取真实 IP


mkdir -pv /var/lib/nginx/tmp/
主要用于存放 Nginx 运行时的临时文件。


```


>
>Nginx 启动时，会以 配置文件里 `user` 指定的用户 来运行 worker 进程。
> 在源码编译安装或系统包安装中，通常 `nginx.conf` 里会有一行：
            `user nginx;`
>因为 Nginx 的 worker 进程是以 `nginx` 用户身份运行的，确保权限安全和文件访问权限正确。
>