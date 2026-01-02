---
date: "20250829"
docx:
  - nginx服务部署.docx(续)
  - nginx优化.docx
实验: 已做
---
# nginx服务部署

## 笔记

==编译安装nginx（续）==

>nginx运行控制
nginx -t  \#检测主配置文件 
nginx  \#启动
killall nginx    \#停止nginx
进程信号
killall -s HUP nginx   \#重载配置
killall -s QUIT nginx   \#nginx=kill -9 pid
>
nginx启动脚本
vim ==/etc/systemd/system/==nginx.service
```
[Unit]
Description=The Nginx HTTP Server
After=network.target remote-fs.target nss-lookup.target

[Service]
Type=forking
PIDFile=/usr/local/nginx/logs/nginx.pid
ExecStart=/usr/local/nginx/sbin/nginx
ExecReload=/usr/local/nginx/sbin/nginx -s reload
ExecStop=/usr/local/nginx/sbin/nginx -s stop
PrivateTmp=true

[Install]
WantedBy=multi-user.target

systemctl daemon-reload
systemctl start nginx
```
![[622e1ff1f1b1f098fb7a53bb43e529d.jpg]]

==nginx配置文件==
>~/conf/nginx.conf
	![[74a4521ee9648999814ba722889a983.jpg]]
>
	worker_processes  1;		\#工作进程数；一般设为CPU核心数，auto自动检测；worker_processes 决定并发能力，建议设为 auto
	![[fd4f92cb42fec11e704732f7f831bc3.jpg]]

==虚拟主机的应用==

>基于域名，多域名解析到一个ip
基于端口。同域名不同端口
基于ip，多个ip

## 实验

### 虚拟主机
主要配置
基于域名
![[Pasted image 20251010115425.png]]
![[Pasted image 20251010115444.png]]
![[Pasted image 20251010155427.png]]
基于端口：
![[Pasted image 20251010155715.png]]
nginx访问目录：
html/
配置虚拟主机：nginx.conf/http/下的 server部分

![[fb13d5ebe61bda82b4895d265bf4a94.jpg]]

![[020c111c7608bc64d2c3de0ae247e73.jpg]]

![[4b4c34c84508e709ef841c20e2d3277.jpg]]

## 补充

==虚拟主机的本质==

- **虚拟主机（Virtual Host）**指的是服务器上**逻辑上的一套独立的 Web 服务环境**。
    
- 本质上就是 **IP/域名 + 端口 的组合**，用于区分不同的 Web 服务。
- **一般情况**：一个虚拟主机对应一个网站
# nginx优化 
## ==https配置==
主要配置
![[Pasted image 20251010115025.png]]

 **概念**
>https数字证书
CA
TLS/SSL 443
>
 HTTP请求过程
1.TCP/IP协议三次握手
2.客户端向server端发送建立连接请求，
3、server端和客户端互相协商使用的加密协议（SSLv2、SSLv3、TLSv1）和加密算法
4.一般客户端没有证书，server将自己证书发给客户端
5、客户端验证证书完整性和信任性及获取server加密协议公钥，然后客户端传递加密后的对称密码给server.
6、然后server拿着对称密码加密数据给客户端传递
>
 HTTP和HTTPS的区别：
1.申请https证书需要一定的费用
2,https用的是443端口，http用的是80端口
3,http是无状态的，明文传输数据，https对数据进行加密传输
>
https的缺点：
1.连接比较耗时
2.并非百分之百安全可靠
3.必须绑定IP地址，同一个IP地址不能绑定多个域名

**实验**
 ![[762a0d681772f60db67ea25e1b41011.jpg]]

![[31ba8e28fd2b6da8f97622c552c4325.jpg]]
>server {
    listen 443 ssl;               # 监听 443 端口（HTTPS）
    server_name www.example.com;  # 域名
	ssl_certificate     /etc/nginx/ssl/example.crt;   # 证书文件
    ssl_certificate_key /etc/nginx/ssl/example.key;   # 私钥文件
    ssl_protocols TLSv1.2 TLSv1.3;                    # 支持的 TLS 版本
    ssl_ciphers HIGH:!aNULL:!MD5;                     # 加密套件
    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
## ==隐藏版本号==
问题：404显示版本
	server_tokens off;     \#http配置
![[Pasted image 20251010155915.png]]
## ==修改work进程身份==
	user root root;    #全局配置

![[Pasted image 20251010160022.png]]

## ==添加访问密码/目录访问控制==
http-tools
htpasswd -c /usr/local/nginx/.htpasswd linux 123.com
添加在location里
	auth_basic
	auth_basic_user_file
![[Pasted image 20251010160209.png]]
==会报401需要授权==客户端错误
![[cccc9557b744c509281076bbc2e3460.jpg]]
server配置
```
server {
    listen 80;
    server_name example.com;

    location /admin {
        auth_basic "Restricted Area";          # 弹框标题（提示文字）
        auth_basic_user_file /etc/nginx/.htpasswd;  # 密码文件路径
        root /usr/share/nginx/html;            # 实际目录路径
    }

    location / {
        root /usr/share/nginx/html;
        index index.html;
    }
}
```
![[Pasted image 20251010160421.png]]
==deny内的用户，报403没有权限==
添加用户
	加密密码
	openssl passwd 123456 \#默认用的MD5算法
	复制结果到./htpasswd里
![[Pasted image 20251010160924.png]]
## ==页面缓存时间==
设置客户端缓存资源时间，优先使用缓存
	location下的expires
![[Pasted image 20251010161021.png]]

![[2302b529a867ed503e66180c2d9e5f7.jpg]]
## ==nginx长连接==
`keepalive_timeout` 用于设置 HTTP 长连接（Keep-Alive）在空闲状态下保持多长时间后关闭。#http里
client_header_timeout 60  \#读请求头超时时间   408 http里
client_body_timeout           \#请求体超时时间      408 http里
指的是http连接时间，而不是os层面的tcp连接
![[Pasted image 20251010161211.png]]
## ==日志分割==
![[Pasted image 20251010161358.png]]
![[Pasted image 20251010161430.png]]
![[43f268474c00c53cf305cd72fbc213c.jpg]]
## ==更改work进程数==
默认一个nginx work
	work_process   \#全局配置
![[Pasted image 20251010161550.png]]
==最好cpu核心数一致==，可以不一致
![[Pasted image 20251005212107.png]]

## cpu亲和性
work_cpu_affinity         \#全局配置
这里必须与work进程数量一致
![[Pasted image 20251010161606.png]]
## ==网页压缩==
\#http参数
gzip   on      \#开启 Gzip 压缩功能
gzip_min_length 1k    \#指定 最小压缩的响应内容长度
gzip_buffers     4 16K   \#缓存区大小,`64KB` 的内存来进行压缩
gzip_http_version  1.1    \#启用压缩所需的 HTTP 协议版本
gzip_comp_level  2     \#压缩等级，取值 `1–9`。
gzip_types   \#指定哪些 MIME 类型 的响应启用 Gzip 压缩
gzip_vary     \#Accept-Encoding区分压缩/非压缩版本
![[Pasted image 20251010161900.png]]

![[a6e4314aaf777847d9dacba77576472.jpg]]
![[Pasted image 20251006020449.png]]
## ==目录索引==
http里：
 定义限流区域，每秒允许 5 个请求
limit_req_zone $binary_remote_addr zone=one:10m rate=5r/s;
sever里：
limit_rate_after        \#下载不限速大小
limit_rate                  \#限速速度

location /download { 
	charset ;
	**autoindex_exact_size on|off**     \#文件大小是否精确到字节
	**autoindex;**  \#是否开启目录列表显示
	**autoindex_locatime;**
    # 使用上面定义的限流区域
        limit_req zone=one burst=10 nodelay;
    limit_req_status       \#指定状态码
}
![[Pasted image 20251010162051.png]]
![[Pasted image 20251010163311.png]]
![[Pasted image 20251010163344.png]]


![[8dd368a9a2ca6be22292e11056d7d0d.jpg]]
## ==目录别名==
alias html   \#效果是不拼接了 location里

`location /abc/ {     alias /var/www/html/; }`

- 请求 `/abc/file.txt` → 实际路径：
    

`/var/www/html/file.txt`

- **规则**：`alias` 指定的路径直接替换掉 location 匹配的部分
    
- location 中 `/abc/` 被替换成 `/var/www/html/`
-![[Pasted image 20251010163433.png]]
访问
![[Pasted image 20251010163649.png]]
显示的不拼接的页面直接的alias
## ==错误页面==
**error_page** 404 /404.html
location = /404.html{
	root html;
}
![[Pasted image 20251010163817.png]]
![[Pasted image 20251010164126.png]]
乱写访问路径显示错误页面404.html
![[Pasted image 20251010164217.png]]

![[0c1bc022821e4ce4472347a2c736827.jpg]]
## ==版本平滑升级==
源码或二进制安装才能平滑升级

替换二进制nginx
发user2信号，向旧master发  \#开启新的master及其work
发送winch信号，向旧master发  \#关闭旧work
发送quit信号，向旧master发  \#关闭旧master
![[Pasted image 20251010164449.png]]