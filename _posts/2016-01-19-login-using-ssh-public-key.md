---
layout: post
title: 使用PublicKey登录SSH
keywords: Public Key, SSH
description: Login Using SSH Public Key
tags: [ ssh,centos,linux ]
---

> 本地(ubuntu 14.04LTS): 192.168.1.5

> 服务器(CentOS 6.6): 192.168.1.10

1. 首先,要在本地机器上生成密钥.

	{% highlight shell  %}
	$ ssh-keygen -t rsa
	Generating public/private rsa key pair.
	Enter file in which to save the key (/home/ioioj5/.ssh/id_rsa): /home/ioioj5/.ssh/vm01_rsa
	Enter passphrase (empty for no passphrase): 
	Enter same passphrase again: 
	Your identification has been saved in /home/ioioj5/.ssh/vm01_rsa.
	Your public key has been saved in /home/ioioj5/.ssh/vm01_rsa.pub.
	The key fingerprint is:
	90:26:68:1c:e9:b2:7a:a8:38:f4:31:52:fc:d8:eb:f7 ioioj5@dp
	The key's randomart image is:
	+--[ RSA 2048]----+
	|  ..             |
	| ..o   .         |
	| .= . +          |
	|...o o .         |
	| o. +   S        |
	|.o + o           |
	|o.o o .          |
	|= .. . .         |
	|+o  ... .E       |
	+-----------------+
	{% endhighlight %}

以上操作会在~/.ssh/目录下生成两个文件, vm01_rsa与vm01_rsa.pub. 其中后缀是.pub的就是公钥, 另一个是私钥.

注: 如果本地是windows的话, key需要使用相应的ssh客户端生成, 如:putty-gen, git for windows 之类的工具.

	

2. 现在, 需要把本地生成的key中的公钥(public key), 也就是后缀是.pub的key,上传到服务器上.

	{% highlight shell  %}
	$ scp ~/.ssh/vm01_rsa.pub root@192.168.1.10
	{% endhighlight %}

3. 登录服务器, 进行如下操作(把上传的publickey 导入到服务器中).

	{% highlight shell  %}
	$ mkdir ~/.ssh
	$ chmod 700 .ssh
	$ cat vm01_rsa.pub >> ~/.ssh/authorized_keys
	$ rm -rf vm01_rsa.pub
	$ chmod 600 ~/.ssh/*
	{% endhighlight %}

4. 配置服务器端的ssh, 打开publicKey认证.

	{% highlight shell  %}
	$ vi /etc/ssh/sshd_config
	PubkeyAuthentication yes
	{% endhighlight %}

5. 现在就可以使用ssh客户端导入本地生成的私钥(vm01_rsa), 登录服务器.

到现在为止, 登录服务器就可以使用密钥登录了, 但是还有以下几个地方需要配置.

1. ssh-server端口更改

2. ssh-server停止使用传统密码登录




























