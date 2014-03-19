---
layout: post
title: mysql恢复数据工具mysqlbinlog使用
description: mysql恢复数据工具mysqlbinlog使用
tags: [ mysqlbinlog,数据库恢复,mysql ]
---


首先必须确保mysql配置文件中的

[mysqld] log-bin=log_name 开启(其中log_name自己定义)

开启的作用就是开启mysql的二进制日志，然后才可以使用mysqlbinlog工具恢复数据

开启之后通过在mysql中运行：

SHOW BINLOG EVENTS

来确认二进制日志的开启

mysqlbinlog有两种方式来恢复数据：

1.通过指定时间：

	D:Mysqldatalog> mysqlbinlog –start-date="2009-11-27 14:01:00" –stop-date="2009-11-27 14:59:59" log_name.000001 > D:01.txt


2.通过指定位置：

参数说明：

	–start-position=N 从二进制日志中第1个位置等于N参量时的事件开始读。
	–stop-position=N 从二进制日志中第1个位置等于和大于N参量时的事件起停止读。


	D:Mysqldatalog> mysqlbinlog –start-position=123 –end-position=456 log_name.000001 > D:01.txt

关于position的说明：position可以通过执行SHOW BINLOG EVENTS命令来查看 然后进入mysql中执行source 命令 mysql>source D:01.txt 恢复数据完成。

最后说明：mysqlbinlog工具虽然很强大，但是为保数据不丢失最好还是跟备份数据同步使用。这样恢复数据就可以仅从最后一次备份开始到事故发生时间。















