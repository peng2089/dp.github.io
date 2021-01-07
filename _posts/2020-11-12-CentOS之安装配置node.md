---
layout: post
title: CentOS之安装配置node
keywords: node,nodejs,CentOS
description: CentOS之安装配置node
tags: [ node,nodejs,linux ]
---

## 1. nvm
> [nvm](https://github.com/nvm-sh/nvm)是一个node.js的版本管理工具, 可以允许多个不同版本的node同时存在.

```
$ curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.37.0/install.sh | bash
```
```
$ source /root/.bashrc // 安装nvm会往此文件中写入信息
$ echo $NVM_DIR // 验证环境变量是否设置成功
$ nvm --version // 验证nvm是否安装成功
// 添加环境变量 NVM_NODEJS_ORG_MIRROR
$ vi /etc/profile
$ export NVM_NODEJS_ORG_MIRROR=https://npm.taobao.org/mirrors/node
$ source /etc/profile // 使上面的配置生效
$ nvm ls-remote // 查看nvm中node可用版本
```

ps: 安装nvm的时候, 可能会出现nvm下载失败的情况, 可以去[这个网站](https://www.ipaddress.com), 查一下raw.githubusercontent.com的ipv4的地址, 例如, 我现在查到的地址是199.232.96.133, 然后把这个ip跟域名放到hosts文件里面做一下映射, 就可以啦.


## 2. Node.js
> [Node.js](https://nodejs.org/zh-cn/)

```
$ nvm install node // 安装最新版本
$ nvm install 14.15.4 // 安装指定版本, 最新的lts
$ nvm ls // 查看已安装的node的版本
$ nvm ls-remote // 查看nvm提供的可下载版本
$ nvm use v10.23.0 // 使用某个版本
```

安装cnpm且更换镜像为淘宝镜像
```
$ npm install -g cnpm --registry=https://registry.npm.taobao.org
```

也可以不安装cnpm, 而更换npm镜像为淘宝镜像
```
$ npm config set registry https://registry.npm.taobao.org --global
$ npm config set disturl https://npm.taobao.org/dist --global
```

配置npm
```
$ npm config list // 显示npm配置项
$ npm config ls -l // 显示npm所有默认配置
// 需要注意以下两个配置, 这两个文件夹有可能会随着开发不断地变大
$ npm config get prefix // npm全局模块存放位置
$ npm config get cache // npmcache存放位置
```

## 3. Vue.js
> Vue官方提供的一个命令行工具 [Vue CLI](https://cli.vuejs.org/zh/guide/)

```
$ npm install -g @vue/cli
$ vue --version // 检查是否安装正确
$ vue create hello-world // 创建项目hello-world
$ cd hello-world
$ npm run serve // 运行项目
```
![npm run serve]({{ site.url }}/static/images/npm_run_serve.png "npm run serve")

## 附录

### Yarn
> [yarn包管理工具](https://classic.yarnpkg.com/zh-Hans/)

```
$ curl --silent --location https://dl.yarnpkg.com/rpm/yarn.repo | sudo tee /etc/yum.repos.d/yarn.repo
$ yum install -y yarn
```