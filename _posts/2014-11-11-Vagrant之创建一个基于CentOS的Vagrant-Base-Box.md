---
layout: post
title: Vagrant之创建一个基于CentOS的Vagrant Base Box
description: Vagrant之创建一个基于CentOS的Vagrant Base Box
tags: [ Vagrant,CentOS ]
---

### 说明

> 机器A: Win 8.1 x64, IP: 192.168.1.5

> 机器B(虚拟机): CentOS 6.5 x64, IP: 192.168.1.8

> 约定: # 为root用户shell, $ 为非root用户shell, > 为windows cmd

### 下载与安装


- 下载CentOS6.5 x64镜像

- 下载并安装[VirtualBox] (https://www.virtualbox.org/wiki/Downloads) ( VirtualBox-4.3.18-96516-Win)

- 下载并安装[Vagrant] (https://www.vagrantup.com/downloads.html) (vagrant_1.6.5)

### 创建虚拟机

- 虚拟机名称:centos65-x64 

- 禁用声音与USB设备

- 网络:桥接

### 安装CentOS (minimal)

具体请参照相关网上教程

### 配置CentOS

- 关闭selinux

		# vi /etc/sysconfig/selinux
		SELINUX=disabled #增加
		:wq #保存，关闭
		# shutdown -r now #重启系统


- 网卡自动开启并通过dhcp获得ip

		# vi /etc/sysconfig/network-scripts/ifcfg-eth0
		ONBOOT=yes
		NM_CONTROLLED=yes
		BOOTPROTO=dhcp

- 安装wget
	
		# yum install wget

- 更改镜像源

	各位可根据自己具体情况选择不同的源, 或者网易源或者阿里源, 此处采用的是阿里源.

		# wget -O /etc/yum.repos.d/CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-6.repo
		# yum clean all
		# yum makecache
		# yum update

- 安装vim

		# yum install vim

- 安装ssh

	默认应该已安装,需确认.

		# yum install openssh-server
		# service sshd start 
		# chkconfig sshd on

- 安装库

		# yum install nano gcc bzip2 make kernel-devel-`uname -r` perl
	
	如果不安装perl,下面在安装VBoxGuestAdditions的时候会报错.

- 更改hostname为 centos65-x64

	如果在安装CentOS的过程中已经设置过,此处略过

		# vi /etc/hosts
		# 
		# vi /etc/sysconfig/network

- 添加用户组admin与用户vagrant

		# useradd vagrant
		# passwd vagrant
		# groupadd admin
		# usermod -G admin vagrant

- 更改sudoers file,实现非root用户vagrant sudo后不需要密码

		#Add SSH_AUTH_SOCK to the env_keep option
		#Comment out the Defaults requiretty line
		#Add the line %admin ALL=NOPASSWD: ALL
		
		# visudo
		
		Defaults    env_keep += SSH_AUTH_SOCK
		
		# 在root    ALL=(ALL)       ALL下
		root    ALL=(ALL)       ALL
		%admin  ALL=(ALL) NOPASSWD:   ALL
		Defaults:vagrant !requiretty

	更改完成后退出root,su进入vagrant用户,并执行 sudo ls,查看是否能执行

![在root家目录下执行sudo ls]({{ site.url }}/static/images/QQ截图201411111729173.png "在root家目录下执行sudo ls")

如上图所示,则表示配置正确.


- 为了vagrant用户有执行ssh的权限, 添加vagrant's public key

		$ mkdir .ssh
		$ curl -k https://raw.github.com/mitchellh/vagrant/master/keys/vagrant.pub > .ssh/authorized_keys
		$ chmod 0700 .ssh
		$ chmod 0600 .ssh/authorized_keys

	此处完成后需要确认~/.ssh/authorized_keys内容不为空.我使用curl老是下载不下来,用的wget.

- 安装VBoxGuestAdditions

		$ wget http://download.virtualbox.org/virtualbox/4.3.8/VBoxGuestAdditions_4.3.8.iso
		$ sudo mkdir /media/VBoxGuestAdditions
		$ sudo mount -o loop,ro VBoxGuestAdditions_4.3.8.iso /media/VBoxGuestAdditions
		$ sudo sh /media/VBoxGuestAdditions/VBoxLinuxAdditions.run
		$ rm VBoxGuestAdditions_4.3.8.iso
		$ sudo umount /media/VBoxGuestAdditions
		$ sudo rmdir /media/VBoxGuestAdditions



## vagrant创建box

> 以下操作在机器A上进行

- 关闭虚拟机

- VboxManage实现宿主机到虚拟机的端口转发

		> VBoxManage modifyvm "centos65-x64" --natpf1 "guestssh,tcp,,2222,,22"

- 创建box

	进入到CentOS65-x64虚拟机的目录,执行一下命令,生成box文件,生成过程会在几分钟内完成.

		> vagrant package --output centos65-x64.box --base centos65-x64

![vagrant package]({{ site.url }}/static/images/QQ截图20141111172917.png "vagrant package")

**注**: 
1.centos65-x64.box为box的名称,base后面跟的是当前虚拟机所在的目录名称
2.在执行以上命令前,需要打开虚拟机.执行上面的命令后,虚拟机会关闭然后进入生成box流程.如果不事先打开虚拟机,生成box的过程可能会报错.


- 创建Vagrant 虚拟机

		> cd e:/vagrant/centos65-x64
		> vagrant init # 在目录下生成Vagrantfile配置文件,修改配置文件
		# Vagrantfile 配置文件
		config.vm.box = "centos65-x64" # 配置vagrant vm 名称,要与box名称相同
		config.vm.network "forwarded_port", guest: 80, host: 8080
		config.vm.synced_folder "d:/www", "/data" # 把本地d盘下的www映射到虚拟机中的/data下

		> vagrant box add centos65-x64 e:/box/centos65-x64.box # 添加box到虚拟机

![添加box到虚拟机]({{ site.url }}/static/images/QQ截图201411111729171.png "添加box到虚拟机")
提示创建成功.

	> vagrant up # 启动虚拟机

![启动vagrant虚拟机]({{ site.url }}/static/images/QQ截图201411111729172.png "启动vagrant虚拟机")

	> vagrant reload # 重新加载Vagrantfile配置文件

	> vagrant suspend # 暂停虚拟机

	> vagrant resume # 恢复虚拟机

	> vagrant halt # 关闭虚拟机

	> vagrant destroy # 删除销毁虚拟机


## 测试

通过ssh客户端工具(如:xshell,putty)连接一下刚配好的虚拟机,配置如下:

	IP: 127.0.0.1
	PORT: 2222
	username: vagrant
	password: vagrant

提示可以连接,到现在整个vagrant 虚拟机的安装过程结束, 接下来就可以根据各自的要求对vagrant虚拟机进行更加细致的配置了,如配置web服务器.

### PS

    > vagrant up

在出现mount share folders后 应该就结束了,但是今天启动后 却发现出现了疑似警告信息

    ==> default: Machine already provisioned. Run `vagrant provision` or use the `--provision`
    ==> default: to force provisioning. Provisioners marked to run always will still run.

后来在github上vagrant的issue中发现了以下内容

![vagrant issue]({{ site.url }}/static/images/QQ截图20141112163143.png "vagrant issue")

So it is ,from now on we should use the commond :

    > vagrant --provision

to up the vagrant virtual machine.