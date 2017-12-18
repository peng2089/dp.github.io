---
layout: post
title: CentOS7 之 防火墙(firewall)配置
keywords: CentOS7, linux
description: CentOS7 之 防火墙(firewall)配置
tags: [ CentOS7, linux ]
---

1. 查看Firewall 服务状态

	```
	# systemctl status firewalld
	```

2. 查看Firewall 的状态

	```
	# firewall-cmd --state
	```

	注意：firewalld默认配置文件有两个：/usr/lib/firewalld/ （系统配置，尽量不要修改）和 /etc/firewalld/ （用户配置地址）

3. 开启、重启、关闭、firewalld.service服务
	```
	# systemctl start firewalld.service # 开启
	# systemctl restart firewalld.service # 重启
	# systemctl stop firewalld.service # 关闭
	```

4. 查看防火墙规则
	```
	# firewall-cmd --list-all
	# firewall-cmd --zone=public --list-ports // 查看所有打开的端口
	```
5. 查询、开放、关闭端口
	```
	# firewall-cmd --query-port=8080/tcp # 查询端口是否开放
	# firewall-cmd --zone=public --add-port=80/tcp --permanent  # 开放80端口
	# firewall-cmd --permanent --remove-port=8080/tcp # 移除端口
	# firewall-cmd --reload # 重新载入防火墙配置(修改配置后要重启防火墙)
	
	# 参数解释
	1. firwall-cmd：是Linux提供的操作firewall的一个工具；
	2. --permanent：表示设置为持久；
	3. --add-port：标识添加的端口；
	```


6. 限制只允许指定的ip可以访问（也可以做内网之间不限制，外网不能访问）
	```
	# firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="172.16.255.113/32" port protocol="tcp" port="3306" accept"
	```

7. 移除这条策略
	```
	# firewall-cmd --permanent --remove-rich-rule 'rule family=ipv4 source address=172.16.255.113/32 port port=3306 protocol=tcp accept'
	```

8. 开机禁用或者启用firewall
	```
	# systemctl disable/enable firewalld.service
	```


## PS

1. zone概念：
	```
	drop – 丢弃所有传入的网络数据包并且无回应，只有传出网络连接可用。
	block — 拒绝所有传入网络数据包并回应一条主机禁止的 ICMP 消息，只有传出网络连接可用。
	public — 只接受被选择的传入网络连接，用于公共区域。
	external — 用于启用了地址伪装的外部网络，只接受选定的传入网络连接。
	dmz — DMZ 隔离区，外部受限地访问内部网络，只接受选定的传入网络连接。
	work — 对于处在你工作区域内的计算机，只接受被选择的传入网络连接。
	home — 对于处在你家庭区域内的计算机，只接受被选择的传入网络连接。
	internal — 对于处在你内部网络的计算机，只接受被选择的传入网络连接。
	trusted — 所有网络连接都接受。
	```

	```
	# firewall-cmd --get-zones // 列出所有可用的区域
	# firewall-cmd --get-default-zone // 列出默认的区域 
	# firewall-cmd --set-default-zone=dmz // 改变默认的区域
	```