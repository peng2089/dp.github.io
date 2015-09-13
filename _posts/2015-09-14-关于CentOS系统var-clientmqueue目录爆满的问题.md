---
layout: post
title: 关于CentOS系统/var/clientmqueue目录爆满的问题
keywords: clientmqueue
description: 爬虫设计
tags: [ clientmqueue, CentOS ]
---

## 问题

今天进行服务器例行检查的时候,突然发现/var/目录快满了,刚开始还以为是日志或者邮件导致的,后来进去查看了一下, 是/var/spool/clientmqueue这个目录的问题,但是什么导致的却不清楚, 去网上搜了一下才知道答案.

## 说明

Crontab 默认会将定时执行的结果通过mail返回给用户。 如果没有启动Sendmail服务， 系统（确切的应该是sendmail程序）将会把每一个结果保存为一个文件，放置在/var/spool/clientmqueue下。


## 导致问题出现的原因

最近有个关于拍卖的网站刚上线,因为需要每隔一段时间运行一段脚本来处理未完成的订单,有个同事偷了个懒,把这个处理过程放到了网站首页,然后让我把这个首页加到crontab中去.加上了目的肯定是达到了.但是出现了另外一个问题:因为这种方式运行肯定是有输出的,结果就是导致了前面出现的情况.

这是crontab中的那条命令

	*/5 * * * * /usr/bin/curl http://www.test.com

## 解决

1. 这是网上的解决方法, 但是不知道什么原因,我这边没用, 依然输出

	*/5 * * * * /usr/bin/curl http://www.test.com > /dev/null 2>&1

2. 我的解决办法: 先创建一个shell脚本文件,然后把命令写入这个shell脚本中,然后在把这个shell脚本添加到crontab中,然后再导向到"无底洞" /dev/null.

- shell脚本order.sh

		> # touch /root/shell/order.sh
		> # vi /root/shell/order.sh
		# 脚本内容
		#! /bin/bash
		/usr/bin/curl http://www.test.com

- crontab

		*/5 * * * * /root/shell/order.sh > /dev/null 2>&1


	这样就没有输出了,也没有邮件了.

3. 还剩最后一个问题: **如何清空这个目录?**

	find /var/spool/clientmqueue/ -type f -exec rm {} \;

到此结束.