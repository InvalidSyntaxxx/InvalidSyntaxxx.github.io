---
title: Nginx原理剖析（一）
tags:
  - Linux
  - Nginx
id: '1213'
categories:
  - - 专业技术
  - - 学习笔记
date: 2023-03-08 23:59:41
---

[http://www.aosabook.org/en/nginx.html](http://www.aosabook.org/en/nginx.html)

[https://www.kancloud.cn/kancloud/master-nginx-develop/51798](https://www.kancloud.cn/kancloud/master-nginx-develop/51798)

## Nginx概述

Nginx 是一个 `模块化`、`事件驱动`、`异步`、`单线程`和`非阻塞` 的 Web服务器，大量使用 **多路复用** 和 **事件通知** ，并将特定任务用于单独进程。Nginx `worker` 可以处理数千个并发连接和请求。

### Nginx代码结构

```shell
.
├── auto       #自动检测系统环境以及编译相关的脚本
│   ├── cc     #关于编译器相关的编译选项的检测脚本
│   ├── lib    #nginx编译所需要的一些库的检测脚本
│   ├── os     #与平台相关的一些系统参数与系统调用相关的检测
│   └── types  #与数据类型相关的一些辅助脚本
├── conf       #存放默认配置文件，在make install后，会拷贝到安装目录中去
├── contrib    #存放一些实用工具，如geo配置生成工具（geo2nginx.pl）
├── html       #存放默认的网页文件，在make install后，会拷贝到安装目录中去
├── man        #nginx的man手册
└── src        #存放nginx的源代码
    ├── core  #nginx的核心源代码，包括常用数据结构的定义，以及nginx初始化运行的核心代码如main函数
    ├── event  #对系统事件处理机制的封装，以及定时器的实现相关代码
    │   └── modules #不同事件处理方式的模块化，如select、poll、epoll、kqueue等
    ├── http   #nginx作为http服务器相关的代码
    │   └── modules #包含http的各种功能模块
    ├── mail   #nginx作为邮件代理服务器相关的代码
    ├── misc   #一些辅助代码，测试c++头的兼容性，以及对google_perftools的支持
    └── os     #主要是对各种不同体系统结构所提供的系统函数的封装，对外提供统一的系统调用接口
```
<!-- more -->
### Worker模型

**Nginx 不会为每个连接创建一个进程或线程**

Nginx 所做的是检查网络和存储的状态，初始化新连接，将它们添加到运行循环，并异步处理直到完成，此时连接被释放并从运行循环中删除。

Nginx通过`kqueue`、`epoll`和`事件端口`等获得有关入站和出站流量、磁盘操作、Socket读取或写入、超时等的异步反馈

nginx 生成多个`worker` 来处理连接，所以它可以很好地跨多个内核进行扩展？

`worker`对于某些磁盘使用和 CPU 负载模式，应调整Nginx 的数量 ：如果负载模式是 CPU 密集型的——例如，处理大量 TCP/IP、执行 SSL 或压缩——Nginx 的数量应与 CPU 内核的数量相匹配`worker`；如果负载主要受磁盘 I/O 限制——例如，从存储中提供不同的内容集，或大量代理 ，`worker`的数量可能是CPU核心数量的一倍半到两倍。

[Nginx 1.9.11开始增加加载动态模块支持，从此不再需要替换nginx文件即可增加第三方扩展](https://developer.aliyun.com/article/311836)

### Nginx 进程角色

Nginx 在内存中运行多个进程；有一个主进程和多个`worker`进程。在 nginx 1.x 版本中，所有进程都是单线程的。所有进程主要使用 **共享内存** 进行进程间通信。

主进程负责以下任务：

*   读取和验证配置
*   创建、绑定和关闭Socket
*   `worker`启动、终止和维护配置的进程数
*   在不中断服务的情况下重新配置
*   控制不间断的二进制升级（启动新的二进制文件并在必要时回滚）
*   重新打开日志文件
*   编译嵌入式 Perl 脚本

这些`worker`进程接受、处理和处理来自客户端的连接，提供反向代理和过滤功能，并完成 nginx 能够完成的几乎所有其他工作。

\*+ 缓存加载程序进程负责检查磁盘缓存项并使用缓存元数据填充 nginx 的内存数据库。本质上，缓存加载器准备 nginx 实例来处理已经存储在磁盘上专门分配的目录结构中的文件。它遍历目录，检查缓存内容元数据，更新共享内存中的相关条目，然后在一切干净并准备好使用时退出。

\*+ 缓存管理器主要负责缓存过期和失效。它在 nginx 正常运行期间保留在内存中，并在出现故障时由主进程重新启动。

### Nginx配置

nginx 配置保存在许多纯文本文件中，这些文件通常位于`/usr/local/etc/nginx`or 中`/etc/nginx`。主配置文件通常称为 `nginx.conf`

配置文件最初由主进程读取和验证。nginx 配置的编译只读形式可供进程使用，`worker`因为它们是从主进程派生出来的。配置结构由通常的虚拟内存管理机制自动共享。

nginx 配置有几个不同的上下文如：`main`, `http`, `server`, `upstream`, `location`（以及 `mail`邮件代理）用于指令块。上下文永远不会重叠。

### Nginx内幕

大多数特定于协议和应用程序的功能都是由 nginx 模块完成的，而不是核心。

在内部，nginx通过模块的管道或链处理连接。换句话说，每个操作都有一个模块在做相关的工作；例如，压缩、修改内容、执行服务器端包括、通过FastCGI或uwsgi协议与上游应用服务器通信或与memcached通信。