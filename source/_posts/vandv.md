layout: Vagrant
title: 使用Vagrant打造跨平台开发环境
date: 2015-12-03 13:02:04
tags: [Vagrant,VirtualBox,开发环境,虚拟机]
---
[Vagrant](http://vagrantup.com/) 是一款用来构建虚拟开发环境的工具，非常适合 php/python/ruby/java 这类语言开发 web 应用，h我们可以通过 Vagrant 封装一个 Linux 的开发环境，分发给团队成员。成员可以在自己喜欢的桌面系统（Mac/Windows/Linux）上开发程序，代码却能统一在封装好的环境里运行。
<!--more-->
## 安装 VirtualBox
小巧方便还免费，为什么不用？<br />
下载地址:`https://www.virtualbox.org/wiki/Downloads`
## 安装 Vagrant
下载地址：`http://downloads.vagrantup.com/` <br />
下载系统镜像:`http://www.vagrantbox.es/`
## 向 Vagrant 添加镜像
打开终端，例如Git bash
```
$ vagrant box add centos /box/precise64.box
```
`centos`是给vagrant的box起的名字，后面是镜像的路径。
## 初始化环境
创建一个目录作为开发目录，并使用`centos`镜像初始化当前目录。
```
$ cd /myproject         # 切换目录
$ vagrant init centos   # 初始化
$ vagrant up            # 自动环境
```
你会看到终端显示了启动过程，启动完成后，我们就可以用 SSH 登录虚拟机了，剩下的步骤就是在虚拟机里配置你要运行的各种环境和参数了。
```
$ vagrant ssh  # SSH 登录
```
从网上下载的虚拟机镜像并不是复合我们的要求，我们会在系统中添加各种环境。设置好后，我们对环境进行打包：
```
$ vagrant package
```
这样，我们就完成了整个开发环境的配置和打包，将打包后的`box`文件分发给其他人，这样他们就可以使用跟你一模一样的环境了。
<br />妈妈再也不用担心我的环境不一致。。。
## 常用命令
```
$ vagrant init  # 初始化
$ vagrant up    # 启动虚拟机
$ vagrant halt  # 关闭虚拟机
$ vagrant reload  # 重启虚拟机
$ vagrant ssh     # SSH 至虚拟机
$ vagrant status  # 查看虚拟机运行状态
$ vagrant destroy  # 销毁当前虚拟机
```