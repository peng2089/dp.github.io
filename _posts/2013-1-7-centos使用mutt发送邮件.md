---
layout: post
title: CentOS配置mutt发送邮件
description: CentOS配置mutt发送邮件
tags: [ mutt,CentOS,Linux ]
---

### CentOS配置mutt发送邮件
---

**安装与配置**

```
[root@server bin]# wget http://downloads.sourceforge.net/msmtp/msmtp-1.4.16.tar.bz2
[root@server bin]# tar jxvf msmtp-1.4.13.tar.bz2
[root@server bin]# ./configure ——prefix=/usr/local/msmtp
[root@server bin]# make
[root@server bin]# make install
```

**查看配置文件位置**

```
[root@server bin]# ./msmtp ——version
msmtp version 1.4.13
TLS/SSL library： none
Authentication library： built-in
Supported authentication methods：
plain cram-md5 external login
IDN support： disabled
NLS： enabled， LOCALEDIR is /opt/msmtop/share/locale
System configuration file name： /opt/msmtp/etc/msmtprc ——msmtp配置文件
User configuration file name： /root/.msmtprc
Copyright （C） 2007 Martin Lambers and others.
This is free software. You may redistribute copies of it under the terms of
the GNU General Public License .
There is NO WARRANTY， to the extent permitted by law.
```

**配置一下msmtp的配置文件**

```
[root@server etc]# more msmtprc
# Set default values for all following accounts.
defaults
logfile /var/log/msmtp/msmtp.log ——该文件要存在，不然没有日志
# The SMTP server of the provider.
account 163
host smtp.163.com
from your_account@163.com
auth login ——这个要为login，好像on不行
user your_account
password your_password
# Set a default account
account default ： 163
```

到这儿时，最好先测试一下

```
[root@server etc]/opt/msmtp/bin/msmtp asd@gmail.com
hello，test
ctrl+d
[root@server etc]tail -f /var/log/msmtp/msmtp.log 
```

看看有没有成功。然后再进入到上面的邮件中，看看信收到没有.

**配置mutt**

我原来在网上一直看到mutt+msmtp发送邮件，我不想装mutt.（汗啊，后面才发现系统已经装了）。一直在找怎么使用msmtp自己来发邮件，邮件可以发，不过功能实在是太少了。那就用已经安装好了的mutt.

```
[root@server bin]# tail -5 /etc/Muttrc
set sendmail="/var/local/msmtp/bin/msmtp"
set realname="woniu"
set use_from=yes
set editor="vi"
```

现在都已经搞定了，开始测试看看

```
[root@server bin]# echo "测试一下" | mutt -s "管理信息"
```


**乱码问题**:

1.通过locale -a 命令查看是否已经存在GB2312,如不存在请安装语言包

2.修改/etc/sysconfig/i18n文件
```
[root@server bin]#vi /etc/sysconfig/i18n
LANG="zh_CN.gb2312"
SUPPORTED="zh_CN.gb2312:zh_CN:zh"
SYSFONT="latarcyrheb-sun16"
```

3.重启电脑即可.