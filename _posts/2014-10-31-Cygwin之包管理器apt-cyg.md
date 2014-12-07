---
layout: post
title: Cygwin之包管理器 apt-cyg
description: Cygwin之包管理器 apt-cyg
tags: [ Cygwin,apt-cyg ]
---


> apt-cyg: cygwin包管理器


**需安装以下软件包**

	wget
	tar
	gawk
	bzip2

**安装apt-cyg**

	# wget http://apt-cyg.googlecode.com/svn/trunk/apt-cyg -P /bin
	# chmod +x /bin/apt-cyg

**更新源**

	# apt-cyg update

**安装包**

> 使用参数u不必每次都更新源

    # apt-cyg install ping -u
	

**查找包**

	# apt-cyg find php

**设置下载站点和缓存目录**

	# apt-cyg -c /cygdrive/d/downloads/cygwin -m http://mirrors.163.com/cygwin/

也可以把默认的缓存和下载站点存到文件中：

    # vi /bin/apt-cyg
    mirror=http://mirrors.163.com/cygwin
    cache=/cygdrive/d/downloads/cygwin
