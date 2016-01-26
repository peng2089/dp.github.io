---
layout: post
title: ubuntu配置安装LAMP环境
keywords: LAMP,LAMP环境,Ubuntu
description: ubuntu配置安装LAMP环境
tags: [ lamp,ubuntu,Linux ]
---

1.安装LAMP 在终端中输入:

	[root@localhost ~]# sudo apt-get install apache2 php5-mysql libapache2-mod-php5 mysql-server

在运行中会提示设置mysql的root密码.

然后打开浏览器输入:http://localhost就可以看到 It works! This is the default web page for this server. The web server software is running but no content has been added, yet. 安装成功.

2.安装php扩展 安装curl扩展

	[root@localhost ~]# sudo apt-get install php5-curl

安装gd

	[root@localhost ~]# sudo apt-get install php5-gd

基本上都一样,记住安装完后重启apache. 

3.开启apache模块

开启mod_rewrite模块

	[root@localhost ~]# sudo a2enmod rewrite

其他的模块基本类似

4.添加虚拟主机 到/etc/apache2/sites-available目录下新建doc文件 文件内容为:

	ServerName doc.local
	DocumentRoot “/var/www/doc/”
	ErrorLog “/var/log/apache2/doc_errors.log”
	CustomLog “/var/log/apache2/doc_accesses.log” common

然后在终端运行

	[root@localhost ~]# sudo a2ensite doc
	[root@localhost ~]# sudo /etc/init.d/apache2 reload

在/etc/hosts中添加 127.0.0.1 doc.local

然后在浏览器中访问http://doc.local

如果提示没有权限 请修改doc目录的权限

5.开启模块

1>.mod_expires 

	[root@localhost ~]# sudo a2enmod expire

然后在站点配置文件中添加如下代码:

	ExpiresActive On
	ExpiresDefault "access plus 1 hour"
	ExpiresByType text/css "access plus 1 hour"
	#ExpiresByType text/html M604800
	#ExpiresByType text/css A2592000
	#ExpiresDefault "access plus 1 hour"

2>.mod_deflate mod_headers 

这两个基本上是要同时开启的.

	[root@localhost ~]# sudo a2enmod headers
	[root@localhost ~]# sudo a2enmod deflate

然后在站点配置文件中添加如下代码:

	#压缩比率:数字越大压缩越厉害,消耗的cpu资源也越多
	DeflateCompressionLevel 9
	#压缩html txt javascript css
	AddOutPutFilterByType DEFLATE text/html text/plain application/x-javascript text/css

6.安装xhprof

首先下载xhprof：

	[root@localhost ~]# wget http://pecl.php.net/get/xhprof-0.9.2.tgr.gz
	[root@localhost ~]# tar -xzvf xhprof-0.9.2.tar.gz

	[root@localhost ~]# cd xhprof-0.9.2
	#copy目录到web目录
	[root@localhost ~]# cp -r xhprof_html /var/www/xhprof

	[root@localhost ~]# cd extension
	#生成confirgure文件
	[root@localhost ~]# phpize #此处注意,如果显示无此命令,需要安装php5-dev
	#编译安装
	[root@localhost ~]# ./confirgure

	[root@localhost ~]# make && make install 

将一下文件写入到/etc/php5/apache2/php.ini中

	extension=xhprof.so
	;#
	;# directory used by default implementation of the iXHProfRuns
	;# interface (namely, the XHProfRuns_Default class) for storing
	;# XHProf runs.
	;#
	xhprof.output_dir=/var/log/xhprof/

重启apache，就会在phpinfo中看到xhprof的信息


另外如要以图表形式查看请安装graphviz

apt-get install graphviz

详细文档:

详细文档 ：xhprof文档 [link]

错误:

	error:Could not reliably determine the server’s fully qualified domain name, using 127.0.1.1 for ServerName

或者出现以下错误:

	[warn] NameVirtualHost *:0 has no VirtualHosts

解决办法:

修改 /etc/apache2/httpd.conf 文件

打开终端，输入以下命令：

	[root@localhost ~]# sudo vim /etc/apache2/httpd.conf

默认情况下，这个是一个空文件，在文件中加入以下内容：

	ServerName localhost

保存文件退出，再次重启apache，错误提示没有了。 


[link]:http://www.162cm.com/p/xhprofdoc.html



