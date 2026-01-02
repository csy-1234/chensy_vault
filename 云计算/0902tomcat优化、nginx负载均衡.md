---
date: "20250902"
docx:
  - tomcat安全优化.docx
  - nginx负载均衡
---
[tomcat优化.docx](附件/tomcat/9-tomcat安全优化.docx)
[nginx负载均衡.doxc](附件/nginx/10-nginx负载均衡.docx)

# Tomcat优化
## 笔记
==监控Tomcat状态==

==Tomcat安全优化==

==定义错误页面==

==默认页面设置==

==Tomat热部署与热加载==

==Tomcat JVM参数优化==





# nginx负载均衡
## 笔记

==代理的概念==

==nginx代理类型==

==nginx反向代理tomcat==

==Nginx负载均衡算法==

==Nginx负载均衡调度状态==


## 实验
### nginx反向代理tomcat
两个tomcat
nginx：
![[Pasted image 20251012140757.png]]
==nginx代理apache==
![[Pasted image 20251012145023.png]]
## 补充
nginx、appache、tomcat都可以做web服务，三者的根目录：

| 软件     | 默认网站根目录                     | 配置文件位置                                         | 配置项                        |
| ------ | --------------------------- | ---------------------------------------------- | -------------------------- |
| Nginx  | $Nginx/html                 | /etc/nginx/nginx.conf 或 $nginx/conf/nginx.conf | `root`                     |
| Tomcat | $CATALINA_HOME/webapps/ROOT | $CATALINA_HOME/conf/server.xml                 | `<Host appBase="webapps">` |
| Apache | /var/www/html (RHEL/CentOS) | /etc/httpd/conf/httpd.conf                     | `DocumentRoot`             |
