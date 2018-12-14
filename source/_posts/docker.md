---
title: docker
date: 2018-12-13 09:37:01
tags:
description: 记录docker的一些命令
---
### docker安装
以CentOs7为例 下面命令root权限 没权限自觉加sudo
```bash
#查询本机的docker版本
$ docker -v
Docker version 18.09.0 build 4d60bd4
#卸载已安装的 docker 
$ yum list installed | grep docker
containerd.io.x86_64    1.2.0-3.el7       @docker-ce-edge
docker-ce.x86_64        3:18.09.0-3.el7   @docker-ce-edge
docker-ce-cli.x86_64    1:18.09.0-3.el7   @docker-ce-edge
$ yum remove containerd.io.x86_64
#安装最新版docker
$ yum install docker-ce last
$ docker -v
Docker version 18.09.0,build 4d60db4
```
### docker 常用的命令
docker 服务相关
```bash
# 查看docker状态
$ systemctl stauts docker
# 启动docker
$ systemctl start docker
# 停止docker
$ systemctl stop docker
# 将docker加入开机启动项
$ systemctl enable docker

# 查询所有的容器
$ docker ps -a -s 
```
docker镜像
```bash
# 获取镜像 以Redis为例 方法一  冒号后面为版本号
$ docker pull redis:latest
# 方法二 dockerfile 以后使用后测试
```

docker容器生命周期
```bash 
# 启动一个新的容器  
$ docker run -d -p 6379:6379 --name myredis redis:latest redis-server --appendonly yes
$ docker ps
$ docker exec -it <容器ID> /bin/bash
$ redis-cli
127.0.0.1:6379>ping
PONG
```
-d 后台模式 不写代表为前台模式 
-it 其实是-i -t 组成 有一个伪终端用来访问容器
--name 重命名 后面my_ubuntu0代表别名
ubuntu 镜像名称
/bin/bash  容器执行的命令
