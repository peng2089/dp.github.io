---
layout: post
title: 进程管理工具Supervisord
keywords: supervisord
description: 进程管理工具Supervisord
tags: [ tools ]
---
# 进程管理工具Supervisord

## 前言
在 web 应用部署到线上后，需要保证应用一直处于运行状态，在遇到程序异常、报错等情况，导致 web 应用终止时，需要保证程序可以立刻重启，继续提供服务。
所以，就需要一个工具，时刻监控 web 应用的运行情况，管理该进程。
Supervisor 就是解决这种需求的工具，可以保证程序崩溃后，重新把程序启动起来等功能。

> Supervisor 是一个用 Python 写的进程管理工具，可以很方便的用来在 UNIX-like 系统（不支持 Windows）下启动、重启（自动重启程序）、关闭进程（不仅仅是 Python 进程）。


## 安装

```bash
# via easy_install
$ easy_install supervisor
# via pip
# yum intall epel-release
# yum install python-pip
$ pip install supervisor
```

## 配置

```bash
# 创建配置文件
$ echo_supervisord_conf >> /etc/supervisord.conf

# 修改配置 /etc/supervisord.conf
[include]
files = /etc/supervisord.conf.d/*.conf

# 新建管理的应用
$ cd /etc/supervisord.conf.d
$ vim beepkg.conf

# 配置文件：
[program:beepkg]					; 程序名称，在 supervisorctl 中通过这个值来对程序进行一系列的操作
directory=/opt/app/beepkg			; 程序的启动目录
command=/opt/app/beepkg/beepkg		; 启动命令，与手动在命令行启动的命令是一样的
autostart=true						; 在 supervisord 启动的时候也自动启动
autorestart=unexpected				; 程序异常退出后自动重启
startsecs=5
user=root							; 用哪个用户启动
redirect_stderr=true				; 把 stderr 重定向到 stdout，默认 false
; stdout 日志文件，需要注意当指定目录不存在时无法正常启动，所以需要手动创建目录（supervisord 会自动创建日志文件）
stdout_logfile =/data/log/beepkg.log


stdout_logfile_maxbytes=20MB			; stdout 日志文件大小，默认 50MB
stdout_logfile_backups=20				; stdout 日志文件备份数
environment=PATH="/home/app_env/bin"	; 可以通过 environment 来添加需要的环境变量，一种常见的用法是使用指定的 virtualenv 环境
```

## supervisord 管理

> Supervisord 安装完成后有两个可用的命令行 supervisord 和 supervisorctl，命令使用解释如下

```bash
$ supervisord -c /etc/supervisord.conf    # 初始启动 Supervisord，启动、管理配置中设置的进程。
$ supervisorctl status					  # 查看supervisord状态
$ supervisorctl stop beepkg               # 停止某一个进程(programxxx)，programxxx 为 [program:beepkg] 里配置的值，这个示例就是 beepkg。
$ supervisorctl start beepkg              # 启动某个进程
$ supervisorctl restart beepkg            # 重启某个进程
$ supervisorctl stop groupworker:         # 重启所有属于名为 groupworker 这个分组的进程(start,restart 同理)
$ supervisorctl stop all                  # 停止全部进程，注：start、restart、stop 都不会载入最新的配置文件。
$ supervisorctl reload                    # 载入最新的配置文件，停止原有进程并按新的配置启动、管理所有进程。
$ supervisorctl update                    # 根据最新的配置文件，启动新配置或有改动的进程，配置没有改动的进程不会受影响而重启。
```

## 创建supervisord服务

>  supervisord服务

1. 创建supervisord service文件
```bash
$ touch supervisord.service

[Unit]
Description=Supervisor daemon

[Service]
Type=forking
ExecStart=/usr/bin/supervisord -c /etc/supervisord.conf
ExecStop=/usr/bin/supervisorctl shutdown
ExecReload=/usr/bin/supervisorctl reload
KillMode=process
Restart=on-failure
RestartSec=42s

[Install]
WantedBy=multi-user.target
```

2. 将文件拷贝到/usr/lib/systemd/system/
```bash
$ cp supervisord.service /usr/lib/systemd/system/
```

3. 设置开机启动
```bash
$ systemctl enable supervisord
```

4. 验证是否开机启动
```bash
$ systemctl is-enabled supervisord
```