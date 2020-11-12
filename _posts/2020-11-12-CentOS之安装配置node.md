---
layout: post
title: CentOS之安装配置node
keywords: node,nodejs,CentOS
description: CentOS之安装配置node
tags: [ node,nodejs,linux ]
---

## nvm安装配置
> [nvm](https://github.com/nvm-sh/nvm)
1. 安装
```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash

```

2. 设置环境变量
```
$ source /root/.bashrc
$ echo $NVM_DIR # 验证环境变量是否设置成功
$ nvm --version # 验证nvm是否安装成功
$ vi /etc/profile
$ export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
$ source /etc/profile
$ nvm ls-remote # 查看nvm中node可用版本
```

## node安装配置

1. 安装node
```
$ nvm install node # 安装最新版本
$ nvm install 10.23.0 # 安装指定版本
$ nvm ls # 查看已安装版本
$ nvm ls-remote # 查看nvm提供的可下载版本
$ nvm use v10.23.0 # 使用某个版本
```

2. 安装cnmp
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

3. 更换镜像
```
$ npm config set registry https://registry.npm.taobao.org --global
$ npm config set disturl https://npm.taobao.org/dist --global
```
[node官网](https://nodejs.org/zh-cn/)

## 包管理工具
1. yarn
```
$ curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
$ yum install -y yarn
```
[yarn官方](https://classic.yarnpkg.com/zh-Hans/)
