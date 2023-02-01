---
layout: post
title: CentOS7 之 防火墙(firewall)配置
keywords: CentOS7, linux
description: CentOS7 之 防火墙(firewall)配置
tags: [ CentOS7, linux ]
---

```bash
// 禁用旧版iptables服务
$ systemctl mask iptables
$ systemctl mask ip6tables

// 启用firewalld并设置为开机启动
systemctl start firewalld.service 
systemctl enable firewalld.service
systemctl stop firewalld.service
```

```bash
firewall-cmd --version                          查看版本
firewall-cmd --help                             查看帮助
firewall-cmd --state                            查看状态
firewall-cmd --list-ports                       查看所有打开的端口
firewall-cmd --list-all                         查看所有规则
firewall-cmd --reload                           重载规则
firewall-cmd --get-active-zones                 查看区域信息
firewall-cmd --get-zone-of-interface=enp4s0     查看指定接口所属区域
firewall-cmd --panic-on                         拒绝所有包
firewall-cmd --panic-off                        取消拒绝所有包
firewall-cmd --query-panic                      查看是否拒绝

// 查看防火墙规则
$ firewall-cmd --list-all
$ firewall-cmd --zone=public --list-ports // 查看所有打开的端口

// 查询、开放、关闭端口
$ firewall-cmd --query-port=8080/tcp # 查询端口是否开放
$ firewall-cmd --zone=public --add-port=80/tcp --permanent  # 开放80端口
$ firewall-cmd --permanent --remove-port=8080/tcp # 移除端口
$ firewall-cmd --reload # 重新载入防火墙配置(修改配置后要重启防火墙)

// 参数解释
// 1. firwall-cmd：是Linux提供的操作firewall的一个工具；
// 2. --permanent：表示设置为持久；
// 3. --add-port：标识添加的端口；

// 临时添加规则策略信息
firewall-cmd --add-port=8080/tcp
// 永久添加规则策略信息
firewall-cmd --add-port=8080/tcp --permanent
// 说明：永久添加的配置需要重载识别
firewall-cmd --reload


// 临时添加多个端口信息
firewall-cmd --add-port={80/tcp,8080/tcp}
// 永久添加多个端口信息
firewall-cmd --add-port={80,8080}/tcp  --permanent

firewall-cmd --list-ports
    80/tcp 8080/tcp

# 临时添加规则策略信息 
firewall-cmd --add-service=http 
# 永久添加规则策略信息 
firewall-cmd --add-service=https --permanent

#添加允许访问多个服务信息
firewall-cmd --add-service={http,https} firewall-cmd --add-service={http,https} --permanent

# 查看检查服务放行情况
firewall-cmd --list-services http https

# 移除临时添加的服务规则
firewall-cmd --remove-service={http,https}
```


转发
```
# 把本机的8080转发到服务器B的80
## 开启ip伪装
$ firewall-cmd --zone=public --add-masquerade --permanent
## 端口转发
$ firewall-cmd --zone=public --permanent --add-forward-port=port=8080:proto=tcp:toaddr=B:toport=80 --permanent
$ firewall-cmd --reload
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
$ firewall-cmd --get-zones // 列出所有可用的区域
$ firewall-cmd --get-default-zone // 列出默认的区域 
$ firewall-cmd --set-default-zone=dmz // 改变默认的区域
```