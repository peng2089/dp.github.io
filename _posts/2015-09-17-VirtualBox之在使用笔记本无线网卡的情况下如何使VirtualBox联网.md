---
layout: post
title: VirtualBox之在使用笔记本无线网卡的情况下如何使VirtualBox联网
keywords: VirtualBox,联网, 无线网卡
description: VirtualBox之在使用笔记本无线网卡的情况下如何使VirtualBox联网
tags: [ VirtualBox, 虚拟机 ]
---
## 问题

最近刚换了个公司, 然后公司使用无线网上网,结果笔记本上的虚拟机就连不上网了. 一直使用的都是桥接方式.后来进行了相关查询, 有两种方式可以解决这个问题.


## 解决办法一

1. "我的电脑" -> 右键 "管理" -> 计算机管理(本地)->系统工具->设备管理器 -> 右键 "添加过时硬件" -> 下一步(安装我手动从列表选择的硬件) ->在常见硬件类型中选择"网络适配器" -> 在网络适配器中选择"Microsoft->Microsoft Loopback Adapter" -> 下一步.... 完成


    ![设备管理器]({{ site.url }}/static/images/20150917-01.png "设备管理器")
    ![设备管理器]({{ site.url }}/static/images/20150917-02.png "设备管理器")
    ![设备管理器]({{ site.url }}/static/images/20150917-03.png "设备管理器")
    ![设备管理器]({{ site.url }}/static/images/20150917-04.png "设备管理器")
    ![设备管理器]({{ site.url }}/static/images/20150917-05.png "设备管理器")
    ![设备管理器]({{ site.url }}/static/images/20150917-06.png "设备管理器")



2. "开始"->"控制面板"->"网络和Internet"->"网络和共享中心"->"更改适配器设置"->选择当前使用的无线网络连接-> 共享->选中"允许其他网络用户通过此计算机的Internet链接来网络", 并在下面的家庭网络链接中选中刚刚创建的Mircrosoft Loopback Adapter, 然后 确定.

    ![无线网络连接]({{ site.url }}/static/images/20150917-09.png "无线网络连接")
    ![无线网络连接]({{ site.url }}/static/images/20150917-10.png "无线网络连接")

3. 打开VirtualBox->"设置"->"网络"->链接方式"桥接网卡",界面名称:"Mircrosoft Loopback Adapter". 然后确定.

    ![VirtualBox]({{ site.url }}/static/images/20150917-07.png "VirtualBox")
    ![VirtualBox]({{ site.url }}/static/images/20150917-08.png "VirtualBox")

4. 然后进入虚拟机进行相关设置就可以了.

## 解决办法二:

待续...