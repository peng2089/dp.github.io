---
layout: post
title: 利用ssh的用户配置文件config管理ssh会话
keywords: ssh,config
description: 列表推导式（list comprehension）是利用其他列表创建新列表（类似于数学中的集合推导式）的一种方法。其工作方式类似于for循环.
tags: [ ssh,ssh会话 ]
---

通常利用 ssh 连接远程服务器，一般都要输入以下类似命令：

    ssh user@hostname -p port

如果拥有多个 ssh 账号, 每次链接的时候会非常麻烦.

还好，ssh 提供一种优雅且灵活的方式来解决这个问题，就是利用 ssh 的用户配置文件 config 管理 ssh 会话。ssh 的用户配置文件是放在当前用户根目录下的 .ssh 文件夹里（~/.ssh/config，不存在则新创建一个），其配置写法如下：

    Host    别名
        HostName        主机名
        Port            端口
        User            用户名
        IdentityFile    密钥文件的路径

有了这些配置，就可以这样用 ssh 登陆服务器了：

    ssh 别名

例1:

> 链接虚拟机vm01

    Host    vm01                                                                                                               
        HostName        192.168.1.4
        Port            22
        User            root


    # ssh vm01

例2:

> 链接github ssh

    Host    github
        HostName        github.com
        PreferredAuthentications publickey
        IdentityFile    ~/.ssh/github.pub

    # ssh -T git@github.com

参数说明:

    Host *  # 只对能够匹配后面字串的计算机有效。“*”表示 所有的计算机
    ForwardAgent no # 设置连接是否经过验证代理（如果存在）转发给远程计 算机
    ForwardX11 no # 设置X11连接是否被自动重定向到安全的通道和显示集 （DISPLAY set）
    RhostsAuthentication no # 设 置是否使用基于rhosts的安全验证
    RhostsRSAAuthentication no # 设置是否使用用RSA算法的基于rhosts的安全验证
    RSAAuthentication yes # 设置是否使用RSA算法进行安全验证
    PasswordAuthentication yes #  设置是否使用口令验证
    FallBackToRsh no    #设置如果用ssh连接出现错误是否自动 使用rsh
    UseRsh no # 设置是否在这台计算机上使用“rlogin/rsh”
    BatchMode no # 如果设为“yes”，passphrase/password（交互式输入口令）的提示将被禁止。当不能交互式输入 口令的时候，这个选项对脚本文件和批处理任务十分有用
    CheckHostIP yes # 设置ssh是 否查看连接到服务器的主机的IP地址以防止DNS欺骗。建议设置为“yes”
    StrictHostKeyChecking no # 如果设置成“yes”，ssh就不会自动把计算机的密匙加入“$HOME/.ssh/known_hosts”文件，并且一旦计算机的密匙发生了变化，就 拒绝连接
    IdentityFile ~/.ssh/identity # 设置从哪个文件读取用户的 RSA安全验证标识
    Port 22 # 设置连接到远程主机的端口
    Cipher blowfish # 设置加密用的密码
    EscapeChar ~ # 设置escape字符


