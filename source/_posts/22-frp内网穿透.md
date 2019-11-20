---
title: 22-frp内网穿透.md
date: 2019-11-19 17:45:45
tags:
  - 网络
---

<img src="/images/index/22.png" />
<!--more-->

# 前言

docker 相关的东西暂且不谈(官网大法好)，让我们来研究一下内网穿透(因为我真的不想一直提交代码到服务器上啊喂！)

之前使用了付费的 `natapp` 完成了微信对接，实际上就是租了一个他们的服务器，然后利用他们的服务器来完成穿透

现在有了自己的服务器，也就没必要去花钱了

关于 `内网穿透` 的教程网上大多都挺繁琐的，对于我这种懒癌来说，[frp](https://github.com/fatedier/frp) 真是省心省力

## 逻辑图

![frp 逻辑图](/images/frp流程.png)

## 构建流程

1. 本地主机下载 FileZilla 和 XShell 来操控远程服务器和虚拟机
2. 根据 [frp](https://github.com/fatedier/frp/blob/master/README_zh.md) 文档下载代码和进行配置，接着放入对应的主机中
        ```shell
        # frpc.ini
        [common]
        server_addr = xxx.xxx.xxx.xxx
        server_port = 7000

        [mysql]
        type = tcp
        local_ip = 127.0.0.1
        local_port = 28002
        remote_port = 28002

        # frps.ini
        [common]
        bind_port = 7000
        ```
3. 远程服务器（腾讯云）开放白名单，[firewalld](https://www.centoschina.cn/course/config-centos/9119.html) 记得通过 7000 和 28002 端口
4. 虚拟机安装 [docker](https://docs.docker.com/install/linux/docker-ce/centos/)，同理设置 firewalld 通过 7000 和 28002 端口
5. 虚拟机安装运行 mysql 并映射到 28002 端口
6. 远程服务器和虚拟机都打开 frp 程序
7. 本地主机使用 navicat 连接远程服务器的 28002 端口，检查是否连接成功
