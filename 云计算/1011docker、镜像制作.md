---
date: "1011"
docx:
  - 镜像-容器.docx
  - 镜像制作.docx
实验: 未做
---
[镜像-容器.docx](附件/虚拟化/镜像-容器.docx)
[镜像制作.docx](附件/虚拟化/镜像制作.docx)
# docker
### docker离线安装
需要6个包：
三个主包：
![[Pasted image 20251109235005.png]]
三个依赖包：
![[Pasted image 20251109235017.png]]

### 命令

```
docker

docker info

镜像：

docker pull

docker save -o busybox.tgz busybox

docker load -i busybox.tgz

docker images

docker inspect 镜像ID

docker tag busybox busybox:v1

docker rmi 仓库名称[:标签]

docker rmi $(docker images -q)

容器:

docker ps

docker ps -a

docker create 镜像

docker run [OPTIONS] IMAGE [COMMAND ] [args]

docker start 容器的ID/名称

docker stop 容器的ID/名称

docker run -d nginx

--restart=always

docker pause db01

docker unpause db01

docker run -it 镜像名 /bin/bash

docker exec -it 容器ID/容器名 /bin/bash

docker exec 容器ID/容器名 whoami

-p 81:80  81前面的事外部宿主机端口

p 192.168.80.13:81:80

-p 192.168.80.13::80

-p 192.168.80.13:54:54/udp

p 82:82 -p 443:443

docker port 容器名/容器ID

docker logs 容器名/容器ID

docker volume ls

-v /data

docker inspect -f {{.Mounts}} 9f91c02   # 某个容器的挂载信息

-v /data:/abc  # 左宿主机路径

-v /$HOME/.bash_history:/opt/.bash_history  

-v /data:/data:ro

docker export -o /opt/test.tar 7dddd6699d86

docker commit -p 容器名/ID new_images_name

docker import /opt/test.tar test:v1

cat centos7.tar | docker import - centos7:test

docker rename

docker restart

docker stats

docker top

docker kill

docker rm

docker cp /root/anaconda-ks.cfg 3aa04bfd45a6:/tmp

-e A_OPTS=hehe -e B_OPTS=haha

-m 512M

--cpu-shares 512 --cpus 2 --cpuset-cpus 0,1

device-write-bps

-device /dev/sda:/dev/sda --device-read-bps /dev/sda:1mb
```

### 常用镜像
```
Nacos
docker pull nacos/nacos-server:latest

MySQL
docker pull mysql:8.0

Redis
docker pull redis:7.0

Elasticsearch (ES)
docker pull docker.elastic.co/elasticsearch/elasticsearch:8.11.0
```
# 镜像制作
  
**Dockerfile**

**镜像操作**

**构建镜像**

USER/WORKDIR事例

ADD事例

VOLUME/EXPOSE事例

ENV事例

RUN事例

ENTRYPOINT事例

CMD事例

利用Dockerfile编写简单的nginxWeb镜像




