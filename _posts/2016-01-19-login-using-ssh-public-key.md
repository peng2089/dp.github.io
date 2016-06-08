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

	以上操作会在~/.ssh/目录下生成两个文件, vm01_rsa与vm01_rsa.pub. 其中后缀是.pub的就是公钥, 另一个是私钥.

	注: 如果本地是windows的话, key需要使用相应的ssh客户端生成, 如:putty-gen, git for windows 之类的工具.

	

2. 现在, 需要把本地生成的key中的公钥(public key), 也就是后缀是.pub的key,上传到服务器上.

		$ scp ~/.ssh/vm01_rsa.pub root@192.168.1.10

3. 登录服务器, 进行如下操作(把上传的publickey 导入到服务器中).

		$ mkdir ~/.ssh
		$ cat vm01_rsa.pub >> ~/.ssh/authorized_keys
		$ rm -rf vm01_rsa.pub
		$ chmod 600 ~/.ssh/authorized_keys
		$ chmod 700 .ssh

4. 配置服务器端的ssh, 关闭password登录, 打开publicKey认证, 打开认证文件位置.

		$ vi /etc/ssh/sshd_config
		PasswordAuthentication no
		PubkeyAuthentication yes
		AuthorizedKeysFile .ssh/authorized_keys

5. 重启 sshd 服务

		$ /etc/init.d/sshd restart


6. 现在就可以使用ssh客户端导入本地生成的私钥(vm01_rsa), 登录服务器.

注意: 如果更改了ssh默认端口, 且在防火墙里面有配置, 记得要在防火墙里面开启新的端口号, 要不然就悲剧了...



























