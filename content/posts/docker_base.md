---
title: "docker 隔离方式"
date: 2022-09-23T15:26:07+08:00
draft: true
tags: ["docker"]
categories: ["docker"]
---

### namespace

* 查看当前系统的 namespace

  lsns -t "type"

* 查看某进程的 namespace
  
  ls -la /proc/"pid"/ns/

* 进入某 namespace 运行命令

  nsenter -t "pid" -n ip addr

| namespace 类型 | 隔离资源                         |
| -------------- | -------------------------------- |
| IPC(ipc)       | system V IPC 和 POSIX 消息队列   |
| Network(net)   | 网络设备、网络协议栈、网络端口等 |
| PID(pid)       | 进程                             |
| Mount(mnt)     | 挂载点                           |
| UTS(uts)       | 主机名和域名                     |
| USR(user)      | 用户和用户组                     |

##### pid
* 不同用户的进程就是通过 pid namespace 隔离开的，且不同 namespace 中可以有相同 pid
* 有了 pid namespace，每个 namespace 中的 pid 能够相互隔离

##### net
* 网络隔离就是通过 net namespace 实现的，每个 net namespace 有独立的 network devices ， IP addresses，IP routing tables， /proc/net 目录
* Docker 默认采用 veth 的方式将 continer 中的虚拟网卡同 host 上的一个 docker bridge:docker0 连接在一起
  
##### ipc
* Container 中进程交互还是采用 linux 常见的进程间交互方法（interprocess communication - IPC），包括常见的信号量、消息队列和共享内存
* containcer 的进程间交互实际上还是 host 上具有相同 pid namespace 中的进程间交互，因此需要在 IPC 资源申请时加入 namespace 信息 - 每个IPC 资源有一个唯一的 32 位ID

##### mut
  mut namespace 允许不同 namespace 的进程看到的文件结构不同，这样每个 namespace 中的进程所看到的文件目录就被隔离开了

##### uts 
  UTS（"UNIX Time-sharing System"）namespace允许每个 container 拥有独立的 hostname 和 domain name，使其在网络上可以被视作一个独立的节点而非 host 上的一个进程

##### user
  每个 container 可以有不同的 user 和 group id，也就是说可以在 container 内部用 container 内部的用户执行程序而非 host 上的用户


### Cgroups （资源管控）

* Cgroups（Control Groups）是linux 下用于对一个或一组进程进行资源控制和监控的机制；
* 可以对诸如 CPU 使用时间、内存、磁盘I/O 等进程所需的资源进行限制；
* 不同资源的具体管理工作由相应的 Cgroup（Subsystem）来实现；
* 针对不同的类型的资源限制，只要将限制策略在不同的子系统上进行关联即可；
* Cgroups 在不同的系统资源管理子系统中以层级树（Hierarchy）的方式来组织管理：每个Cgroup都可以包含其他的Cgroup，因此Cgroup能使用的资源出了受本 Cgroup 配置的资源参数限制，还受到 父Cgroup 设置的资源限制；

