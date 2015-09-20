---
layout: post
title: iptables配置与使用
keywords: iptables,iptables配置使用,CentOS
description: iptables配置与使用
tags: [ iptable,centos,linux ]
---

简介:iptables 是与最新的 2.4.x 版本 Linux 内核集成的 IP 信息包过滤系统。如果 Linux 系统连接到因特网或 LAN、服务器或连接 LAN 和因特网的代理服务器， 则该系统有利于在 Linux 系统上更好地控制 IP 信息包过滤和防火墙配置。 —来自百度百科

# 开启与关闭

	#重启生效
	[root@vm01 shell]# chkconfig iptables on
	[root@vm01 shell]# chkconfig iptables off
	#即时生效,重启后失效
	[root@vm01 shell]# service iptables start
	[root@vm01 shell]# service iptables stop
	[root@vm01 shell]# service iptables restart
	# 或者
	[root@vm01 shell]# /etc/init.d/iptables start
	[root@vm01 shell]# /etc/init.d/iptables stop
	[root@vm01 shell]# /etc/init.d/iptables restart

# 配置

## 查看本机iptables规则

	[root@vm01 shell] iptables -L -n

## 清除规则

	[root@vm01 shell]# iptables -F        #清除预设表filter中的所有规则链的规则
	[root@vm01 shell]# iptables -X        #清除预设表filter中使用者自定链中的规则


**封单个IP**

	iptables -I INPUT -s ***.***.***.*** -j DROP

**封IP段**

	iptables -I INPUT -s ***.1.0.0/16 -j DROP
	iptables -I INPUT -s ***.2.0.0/16 -j DROP
	iptables -I INPUT -s ***.3.0.0/16 -j DROP

封整个段的命令是

	iptables -I INPUT -s 211.0.0.0/8 -j DROP

封几个段的命令是

	iptables -I INPUT -s 61.37.80.0/24 -j DROP
	iptables -I INPUT -s 61.37.81.0/24 -j DROP

服务器开机启动自运行

把它加到/etc/rc.local中,也可以把你当前的iptables规则放/etc/sysconfig/iptables中，系统启动iptables时自动执行. 后种更好,一般iptables服务会在network服务之前启来，更安全


**解封**

	iptables -L INPUT
	iptables -L --line-numbers #查看规则序号 
	iptables -D INPUT 序号


**开放端口**

	iptables -I INPUT -p tcp --dport 3690 -j ACCEPT ##开放3690端口
	service iptables save 
	service iptables restart

**关闭端口**

	iptables -I INPUT -p tcp –dport 3690 -j DROP ##关闭3690端口
	service iptables save
	service iptables restart


[Iptables 指南 1.1.19][link]

[link]:https://www.frozentux.net/iptables-tutorial/cn/iptables-tutorial-cn-1.1.19.html