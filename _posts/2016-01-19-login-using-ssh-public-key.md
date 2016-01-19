---
layout: post
title: Login Using SSH Public Key
keywords: Public Key, SSH
description: Login Using SSH Public Key
tags: [ ssh,centos,linux ]
---

> local(ubuntu 14.04LTS): 192.168.1.5
> server(CentOS 6.6): 192.168.1.10

1. The first thing you need to do is generate a key.

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

now, you have two files, one public key with .pub suffix(vm01_rsa.pub), and another one is private key(vm01_rsa), under ～/.ssh directory.

2. So now, you need to copy public key(vm01_rsa.pub) to the remote machina(server)

	$ scp ~/.ssh/vm01_rsa.pub root@192.168.1.10

3. login server

	$ mkdir ~/.ssh
	$ chmod 700 .ssh
	$ cat vm01_rsa.pub >> ~/.ssh/authorized_keys
	$ rm -rf vm01_rsa.pub
	$ chmod 600 ~/.ssh/*


4. open Pubkey authentication in server.

	$ vi /etc/ssh/sshd_config
	PubkeyAuthentication yes

5. close pam authentication in server.

	UserPAM no


That's it!





























