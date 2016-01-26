---
layout: post
title: 服务器能ping通,但是ssh,http无法连接
keywords: ssh无法连接,ping通,服务器,
description: 服务器今天出现了问题,如下,能ping通,但是ssh,http都无法连接.
tags: [ 服务器,宕机 ]
---

### 描述

> 服务器宕机, 网络能ping通,但是ssh,http无法连接.
> 宕机时间从8:57:32到9:54:34
> CentOS 5.4 x64
> 2014-9-26 8:57:32

### 问题出现之前的操作

    对服务器操作系统进行了更新 ( CentOS 5.10 Final x64 )

### 判断可能原因


**ssh vsftpd 暴力破解**

    # cat /var/log/secure | grep 'Invalid user' # 不存在跟没有认证的用户的ssh登录尝试
    # cat /var/log/secure | grep -v invalid | grep 'Failed password' # 授权过的但是密码不对ssh登录尝试
    # cat /var/log/secure | grep 'incorrect password' # 授权过的但是密码不对vsftpd登录尝试


**开启了IPv6,IPV4与IPV6冲突导致SSH无法绑定端口**

    # ifconfig # 看下面是否存在ipv6

**SSH配置或者没有开启**

    # /etc/init.d/sshd status


**iptables规则**
    
    # iptables -F # 清除预设表filter中所有规则链中的规则。
    # iptables -X # 清除预设表filter中使用者自定链中的规则。
    # iptables -Z
    # service iptables stop #关闭防火墙

    

**selinux开启**

    # vi /etc/selinux/config
    #SELINUX=enforcing #注释掉
    #SELINUXTYPE=targeted #注释掉
    SELINUX=disabled #增加

    shutdown -r now #重启系统

**端口占用**

    # lsof -i tcp:80 # tcp:22
    COMMAND   PID   USER   FD   TYPE DEVICE SIZE/OFF NODE NAME
    httpd    1342 apache    3u  IPv6 514672      0t0  TCP *:http (LISTEN)
    
    ...

    # pkill http #或者 kill -9 1342


**内存耗光**

    # free -m
                 total       used       free     shared    buffers     cached
    Mem:          5961       5414        547          0        752       3850
    -/+ buffers/cache:        811       5150
    Swap:         1992         52       1939

    # cat /var/log/message | grep "out of memory" 

**CPU**

    # top

主要是看进程的cpu/占用内存/运行时间,当然也可以看内存使用以及负载等.

**负载**

    # uptime 
    12:40:11 up  2:47,  1 users,  load average: 0.04, 0.03, 0.08

当然,现在的负载小多了,当时的负载我记得是在16点几.

### 问题出现后的操作

- 重启服务器

- 连接ssh, 检测...

- 停止httpd,vsftpd,mysql等服务.

- 分析http错误日志,发现大量mod_pagespeed错误提示.

- 关闭mod_pagespeed, 保存错误日志后清空错误日志文件

- 重启httpd, 继续查看http错误日志

- 正常

### 初步原因

    服务器内存耗光,apache进程导致cpu飙升到100%

### 深度原因

    mod_pagespeed在更新后自动打开了,在老网站生成缓存的时候出错了.
    但是这个操作几乎无限进行,结果瞬间生成了大量的错误日志写入到了http错误日志中,
    大量的写操作导致宕机


当然,至于为什么mod_pagespeed会出现这种问题,继续寻找中...

EOF