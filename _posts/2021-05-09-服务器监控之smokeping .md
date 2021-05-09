---
layout: post
title: 服务器监控之smokeping
keywords: smokeping
description: 服务器监控之smokeping
tags: [ smokeping ]
---
# 服务器监控之smokeping

1. 安装依赖
```bash
$ sudo apt-get install curl libauthen-radius-perl libnet-ldap-perl libnet-dns-perl libio-socket-ssl-perl libnet-telnet-perl libsocket6-perl libio-socket-inet6-perl apache2
```


2. 安装smokeping
```bash
$ sudo apt-get install smokeping
$ sudo apt-get install sendmail
```

3. 安装tcpping
```bash
$ sudo apt-get install tcptraceroute bc
$ cd /usr/bin
$ sudo wget http://www.vdberg.org/~richard/tcpping
$ sudo chmod +x tcpping
```

4. 配置smokeping
```bash
$ vi /etc/smokeping/config.d/Ping
+ ping
probe = TCPPing
menu = Server
title = Server
++ Log Server
menu = Log
title = Log
host = ***.***.***.***

$ vi /etc/smokeping/config.d/Probes
*** Probes ***

+ FPing
binary = /usr/bin/fping

+ TCPPing
binary = /usr/bin/tcpping
forks = 5
offset = 50%
step = 60
timeout = 15

+ Curl
binary = /usr/bin/curl
forks = 24
offset = 90%
agent = User-Agent: Smokeping/2.6.9
expect = OK
extraargs = -4 --head
extrare = / /
follow_redirects = no
include_redirects = no
insecure_ssl = 1
interface = eth0
timeout = 20
urlformat = http://%host%/
```

5. 配置apache
```bash
$ sudo vim /etc/apache2/conf-available/serve-cgi-bin.conf

ScriptAlias /smokeping/smokeping.cgi /usr/lib/cgi-bin/smokeping.cgi
Alias /smokeping /usr/share/smokeping/www
<Directory "/usr/share/smokeping/www">
	Options FollowSymLinks
</Directory>
```

6. 重启服务
```bash
sudo a2enmod cgi
sudo service apache2 restart
sudo service smokeping restart
```


7. 附录
https://oss.oetiker.ch/smokeping/doc/smokeping_install.en.html
https://blog.csdn.net/chijiu4353/article/details/100643709
https://blog.csdn.net/weixin_33725126/article/details/85589365
https://blog.csdn.net/bittersweet0324/article/details/77585368
https://blog.csdn.net/XYliurui/article/details/103107632