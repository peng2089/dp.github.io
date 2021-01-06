---
layout: post
title: CentOS之安装配置node
keywords: node,nodejs,CentOS
description: CentOS之安装配置node
tags: [ node,nodejs,linux ]
---

## nvm安装配置
1. 安装
```bash
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
```

2. 设置环境变量
```bash
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
$ nvm install 14.15.4 # 安装指定版本, 最新的lts
$ nvm ls # 查看已安装版本
$ nvm ls-remote # 查看nvm提供的可下载版本
$ nvm use v10.23.0 # 使用某个版本
```

2. 安装cnmp
```bash
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

3. 更换镜像
```bash
$ npm config set registry https://registry.npm.taobao.org --global
$ npm config set disturl https://npm.taobao.org/dist --global
```

4. 配置npm
```bash
$ npm config list # 显示npm配置项
$ npm config ls -l # 显示npm所有默认配置
# 需要注意以下两个配置, 这两个文件夹有可能会随着开发不断地变大
$ npm config get prefix # npm全局模块存放位置
$ npm config get cache # npmcache存放位置
```

## 包管理工具
1. yarn
```bash
$ curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
$ yum install -y yarn
```

## 附录
[node官网](https://nodejs.org/zh-cn/)

[yarn官方](https://classic.yarnpkg.com/zh-Hans/)

[nvm](https://github.com/nvm-sh/nvm)