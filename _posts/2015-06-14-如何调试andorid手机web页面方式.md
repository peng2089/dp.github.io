---
layout: post
title: 如何调试andorid手机web页面方式
keywords: android,调试
description: 如何调试手机web页面方式
tags: [ 调试,android ]
---

调试手机端web页面

## 平台

> ubuntu 15.04 x64 desktop

> IP:192.168.1.5

## 准备

1. 手机端: chrome for android 

2. 电脑端: chrome 43+

3. 手机USB线

## 开启手机的调试模式

## 查找设备并调试

chrom -> menu -> more tools -> inspect devices

![inspect devices]({{ site.url }}/static/images/inspect-devices.png "inspect devices")

如上图所示, Che1-CL20 就是我的手机设备.这个时候打开手机上的chrome,然后返回 当前页面 刷新一下,就会出现以下页面.

![reload inspect devices]({{ site.url }}/static/images/reload-inspect-devices.png "reload inspect devices")

此时你就可以在 open 前面的那个输入框中输入 你要测试的ip，就可以在手机上看到要测试的页面.

这里要说明下，上面提到测试的时候要输入ip，如果这个地方你想输入你在电脑上设置的虚拟域名也是可以的. 有两种方式实现:

1. android ROOT后，修改手机上的hosts文件，然后映射到电脑上设置的虚拟域名

2. 局域网中搭建一台DNS服务器，然后在路由器 -> DHCP服务器中把DNS服务器的地址设置为局域网中的dns服务器地址,然后重启路由器.这样就不用在手机中修改hosts文件了.

![dns]({{ site.url }}/static/images/dns.png "dns")

注意: 要修改一下本机的dns,指定为局域网中的dns服务器地址.因为我是在本机中安装的dns服务器,所以dns地址设置为本机的ip即可.

	$ sudo vi /etc/resolv.conf

	nameserver 192.168.1.5


最终效果 
![最终效果]({{ site.url }}/static/images/phone.jpeg "最终效果")


## 安装并配置DNS服务器

	$ sudo apt-get install bind9 bind9-doc dnsutils

在默认情况下,bind9是被配置为DNS缓存服务器来使用的.因此只要把DNS服务器地址加入转发列表就行.


	$ sudo vi /etc/bind/named.conf.local

	zone "novel.local" {
		type master;               
		file "/etc/bind/db.novel.local";
	};
	zone "192.168.1.in_addr.arpa" {
		type master;
		file "/etc/bind/db.192.168.1";
	};

	$ sudo vi /etc/bind/name.novel.local

	;
	; BIND data file for local loopback interface
	;
	$TTL	604800
	@	IN	SOA	novel.local. root.novel.local. (
					  2		; Serial
				 604800		; Refresh
				  86400		; Retry
				2419200		; Expire
				 604800 )	; Negative Cache TTL
	;
	@	IN	NS	novel.local.
	@	IN	A	192.168.1.5

	*.novel.local 14400 IN A 192.168.1.5

	$ sudo vi /etc/bind/db.192.168.1

	;
	; BIND reverse data file for broadcast zone
	;
	$TTL	604800
	@	IN	SOA	novel.local. root.novel.local. (
					  1		; Serial
				 604800		; Refresh
				  86400		; Retry
				2419200		; Expire
				 604800 )	; Negative Cache TTL
	;
	@	IN	NS	novel.local.

	134 IN PTR http://nvel.local


	$ sudo vi /etc/bind/name.local.options

	forwarders {
		202.106.0.20;
	};


