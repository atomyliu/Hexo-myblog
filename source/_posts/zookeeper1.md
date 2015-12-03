layout: zookeeper
title: 虚拟集群搭建zookeeper环境初探
date: 2015-12-03 14:33:04
tags: [zookeeper,集群]
---
ZooKeeper是一个分布式开源框架，提供了协调分布式应用的基本服务，它向外部应用暴露一组通用服务——分布式同步（Distributed Synchronization）、命名服务（Naming Service）、集群维护（Group Maintenance）等，简化分布式应用协调及其管理的难度，提供高性能的分布式服务。ZooKeeper本身可以以Standalone模式安装运行，不过它的长处在于通过分布式ZooKeeper集群（一个Leader，多个Follower），基于一定的策略来保证ZooKeeper集群的稳定性和可用性，从而实现分布式应用的可靠性。
有关ZooKeeper的介绍，网上很多，也可以参考文章后面，我整理的一些相关链接。
下面，我们简单说明一下ZooKeeper的配置。
<!--more-->
## 下载安装
```
$ wget http://mirrors.hust.edu.cn/apache/zookeeper/zookeeper-3.4.7/zookeeper-3.4.7.tar.gz   # 下载zookeeper压缩包
$ tar xzcf zookeeper-3.4.7.tar.gz -C ~/zookeeper    # 解压缩到主目录zookeeper文件夹
```
## 配置zoo.cfg文件
```
# 心跳间隔时间(ms)
tickTime=2000
# Follower与Leader初始化时能容忍的时间(initLimit * tickTime)
initLimit=10
# Follower与Leader请求响应时能容忍的时间(syncLimit * tickTime)
syncLimit=5
# server1数据目录，每个server各自配置
dataDir=~/zookeeper
# 客户端连接端口
clientPort=2181
# server列表:
# .n(标记server)=(server地址):(投票时所用端口):(选举Leader时所用端口)
server.1=192.168.10.10:2888:3888
server.2=192.168.10.11:2888:3888
server.3=192.168.10.12:2888:3888
```
在`~/zookeeper`目录下创建myid文件，并标识对应的server
```
$ vi ~/zookeeper/myid
```
在里面输入1 ，测试一下
```
$ cat ~/zookeeper/myid
$ 1
```
## 启动zookeeper
同时配置其他2台虚拟机环境，然后运行
```
$ ./zkServer.sh start-foreground
```
如果不出问题，会连接上，第一个服务器会显示`Follower`
## 客户端连接集群
```
$ ./zkCli.sh -server 192.168.10.10:2181,192.168.10.11:2181,192.168.10.12:2181
```
这样，一个基本的zookeeper集群建立完成。