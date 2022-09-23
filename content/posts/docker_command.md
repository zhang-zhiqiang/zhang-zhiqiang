---
title: "docker 基本概念与命令"
date: 2022-09-23T15:28:41+08:00
draft: false
tags: ["docker"]
categories: ["docker"]
---

[Docker 入门](*()https://www.ruanyifeng.com/blog/2018/02/docker-tutorial.html)

DOCKER 三大概念
* 镜像： 类似于虚拟机镜像、可以理解为一个只读的模版 例如： 一个镜像可以只包含一个基本的操作系统环境，里面仅安装了 apache 应用程序， 可以把它成为 apache 镜像，镜像是创建 docker 容器的基础
* 容器：类似于一个轻量级的沙箱，Docker 利用容器来运行和隔离应用。 容器是从镜像创建的应用程序实例，它可以启动、开始、停止、删除，而这些容器都是相互隔离、互不可见的。（容器是镜像运行实例）
* 仓库： 类似于代码仓库，是 Docker 集中存放镜像文件的场所。


设置镜像代理
[阿里云镜像](https://cr.console.aliyun.com/cn-hangzhou/instances/mirrors)

#### images 常用命令

```bash
# 列出本机的所有 image 文件。
# -a 列出所有包括临时文件
$ docker image ls

# 查找镜像
$ docker search

# 删除 image 文件
# -f 强制删除一个存在容器依赖的镜像，正确的做法应为先删除依赖该镜像的所有容器再删除镜像
$ docker image rm [imageName]

# 清理镜像
# -a 删除所有无用的镜像，不光是临时镜像
$ docker image prune

# 查看镜像详细信息
$ docker [image] inspect

# 下载最新镜像
# imageName 默认从 hub 下载， 可以选择指定 代码仓库地址
$ docker image pull [imageName]
$ docker pull hub.c.163.com/public/ubun七u:18.04

# 创建镜像
# -t 参数用来指定 image 文件的名字
# 后面还可以用冒号指定标签。如果不指定，默认的标签就是latest。
# 最后的那个点表示 Dockerfile 文件所在的路径，
$ docker image build -t koa-demo .

# 导出镜像
# docker save -o ubuntu_18.04.tar ubuntu:18.04

# 载入镜像
$ docker load -i ubuntu_18.04.tar
```

#### container 命令
``` bash
# run 命令具有自动抓取 image 文件的功能。如果发现本地没有指定的 image 文件，就会从仓库自动抓取。
$ docker container run [容器名称]

# 终止
$ docker container kill [containID]

# --all 展示所有(默认隐藏)
$ docker container ls

# 终止运行的容器文件
$ docker container rm [containerID]

# 生成容器
$ docker container run -p 8000:3000 -it koa-demo /bin/bash

# -p 容器的 3000 端口映射到本机的 8000 端口
# -it参数：容器的 Shell 映射到当前的 Shell，然后你在本机窗口输入的命令，就会传入容器。
# -d 后台运行
# -i 打开标准输入接收用户输入命令
# -t 分配伪终端
# -e 指定容器内环境变量
# koa-demo:0.0.1：image 文件的名字（如果有标签，还需要提供标签，默认是 latest 标签）。
# /bin/bash：容器启动以后，内部第一个执行的命令。这里是启动 Bash，保证用户可以使用 Shell。

```

#### 仓库
``` bash
# hub.docker.com
# 本地私有仓库搭建
# -v 参数来将镜像文件存放在本地的 /opt/data/r巳gistry
$ docker run -d -p 5000:5000 registry:2
$ docker run -d -p 5000 5000 -v /opt/data/registry:/var/lib/registry registry:2

# 开源方案： nexus
```

#### Dockerfile 文件
文件内容编写一般分为四个部分
1. 基础镜像信息
2. 维护者信息
3. 镜像操作指令
4. 容器启动时执行指令
``` bash
FROM node:8.4 # 该 image 文件继承官方的 node image，冒号表示标签，这里标签是8.4，即8.4版本的 node。
COPY . /app # 将当前目录下的所有文件（除了.dockerignore排除的路径），都拷贝进入 image 文件的/app目录
WORKDIR /app # 指定接下来的工作路径为/app。
RUN npm install --registry=https://registry.npm.taobao.org # 在/app目录下，运行npm install命令安装依赖。注意，安装后所有的依赖，都将打包进入 image 文件。
EXPOSE 3000 # 将容器 3000 端口暴露出来， 允许外部连接这个端口。
CMD node demos/01.js # 容器启动后自动执行 node demos/01.js
# RUN CMD 区别
# RUN命令在 image 文件的构建阶段执行，执行结果都会打包进入 image 文件；一个 Dockerfile 可以包含多个RUN命令
# CMD命令则是在容器启动后执行, 只能有一个CMD命令。

# run命令是新建容器，每运行一次，就会新建一个容器。同样的命令运行两次，就会生成两个一模一样的容器文件
#重复使用容器，就要使用docker container start命令，用来启动已经生成、已经停止运行的容器文件
$ docker container start [containerID]

# kill 命令发送 SIGKILL 信号 stop 发送 SIGTERM
$ docker container stop [containerID]

# 用来查看 docker 容器的输出，即容器里面 Shell 的标准输出
$ docker container logs [containerID]

# 用于进入一个正在运行的 docker 容器
$ docker container exec -it [containerID] /bin/bash

# 用于从正在运行的 Docker 容器里面，将文件拷贝到本机
$ docker container cp [containID]:[/path/to/file] .

$ docker [container] cp data test:/tmp/

# 查看端口映射
$ docker container port test
```

#### 数据管理
1. 数据卷： 容器内数据直接映射到本地
2. 数据卷容器：使用特定的容器来维护数据卷

``` bash
$ docker create volume
```

#### 容器互联
``` bash
# --link 参数
```

