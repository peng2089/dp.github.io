---
layout: post
title: Cygwin 之 mysql
keywords: mysql,Cygwin
description: 在Cygwin上安装mysql,解决mysql登陆挂起的情况
tags: [ Cygwin ]
---


众所周知,连接mysql的方式是

	mysql -u root -p

本来 mysql 会提示输入密码，但在 Cygwin 中程序会直接挂起，不再响应，实际上即使在 -p 参数后面跟上密码，也是一样的。

解决的办法，使用 Cygwin 版本的 MySQL 客户端，但 Cygwin 并没有提供，所以就只有自己动手编译一个！
{% highlight console  %}
# wget http://dev.mysql.com/get/Downloads/MySQL-5.5/mysql-5.5.0-m2.tar.gz
# tar zxvf mysql-5.5.0-m2.tar.gz
# cd mysql-5.5.0-m2
{% endhighlight %}
在动手编译之前，先打开 Cygwin 安装程序安装 readline，用来替代 MySQL 自带的。MySQL 源码包捆绑的 readline 在 Cygwin中编译会报错。

准备好以后，开始编译过程：
{% highlight console  %}
# ./configure --without-server --without-readline CFLAGS=-O2 CXXFLAGS=-O2
# make
# make install
{% endhighlight %}

编译安装完 MySQL Client，打开 Windows 系统中的 MySQL Server，使用如下的命令测试一下：
{% highlight console  %}
# mysql -h127.0.0.1 -uroot -p
{% endhighlight %}
为什么加上 -h127.0.0.1 呢？默认的情况下，不带 -h 参数或者使用 -h localhost，MySQL 都会使用 Unix socket file 连接服务器，即使你在命令中指定了端口也会被忽略的，所以肯定连接不上的，提示找不到 /tmp/mysql.sock。使用 IP 或者主机名后，MySQL 就会使用 TCP/IP 模式连接服务器的 3306 端口，这样就什么没问题了。

为了方便，在配置文件中强制客户端使用 TCP/IP 连接模式。

复制 mysql-5.5.0-m2/support-files 中的配置文件样板到 /etc/my.cnf，EG：
{% highlight console  %}
# cp support-files/my-medium.cnf /etc/my.cnf
{% endhighlight %}

在 [client] 中加入 protocol=TCP，eg：
{% highlight vim  %}
# The following options will be passed to all MySQL clients
[client]
#password   = your_password
port        = 3306
socket      = /tmp/mysql.sock
protocol    = TCP
 
## 指定客户端连接的默认编码，注意是 utf8，不是 utf-8
## 可根据需要自行修改
default-character-set = utf8
{% endhighlight %}
之后就可以使用 mysql -uroot -p 直接连接 Windows 中的 MySQL Server 了。


来源:http://www.phpvim.net/os/windows/build-mysql-client-on-cygwin.html
