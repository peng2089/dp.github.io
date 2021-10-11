---
layout: default
title: Tools
---

1. [goreplay](https://github.com/buger/goreplay)
> GoReplay 是一个开源网络监控工具，可以将实时 HTTP 流量捕获并重放到测试环境。

2. iftop
> 类似于top的实时流量监控工具

3. ifstat
> interface statistics, 是一个简单的网络流量监测工具。
常用选项
-a: 检测系统上所有网卡接口。
-i：指定要检测的网卡接口。
-t：在每行输出信息前加上时间戳。
-b：以Kbit/s为单位显示数据，而不是默认的KB/s。
delay：采样间隔（默认是秒），即每个delay的时间输出一次统计信息。
count：采样次数，即共输出count次统计信息。

```
$ ifstat -a 2 5 # 每隔两秒输出一次结果，共输出五次。
```

4. netstat
> netstat是一个功能很强大的网络信息统计工具。它可以打印本ID网卡接口上的全部连接、路由表信息、网卡接口信息等。
常用选项包括：
-n：使用IP地址表示主机，而不是主机名；使用数字表示端口号，而不是服务名称。
-a：显示结果中也包含监听socket。
-t：仅显示TCP连接。
-r：显示路由信息。
-i：显示网卡接口的数据流量。
-c：每隔一秒输出一次。
-o：显示socket定时器的信息。
-p：显示socket所属的进程的PID和名字。

```
$ netstat -ant | grep ":8080"
```