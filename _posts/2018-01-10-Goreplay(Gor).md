---
layout: post
title: GoReplay安装与使用
keywords: GoReplay
description: GoReplay安装与使用
tags: [ GoReplay ]
---
> GoReplay is an open-source tool for capturing and replaying live HTTP traffic into a test environment in order to continuously test your system with real data. It can be used to increase confidence in code deployments, configuration changes and infrastructure changes.

https://github.com/buger/goreplay


1. --input-raw: 捕获http流量, 指定ip或者端口
2. --input-file: 流量文件
3. --input-tcp: 
4. --output-http: 重放http流量到一个给定的节点
5. --output-file: 输出流量到文件
6. --output-tcp: 定向流量到另一个Gor实例
7. --output-stdout: 输出流量到标准输出

```
goreplay --input-raw :80 --output-stdout
goreplay --input-raw :80 --output-stdout
goreplay --input-raw :80 --output-stdout --http-allow-header "Host: api.test.local"
goreplay --input-raw :80 --output-file=goreplay.log
goreplay --input-raw :80 --output-http="http://api.test.dev"
```


## 捕获

## 重放

## 性能测试

## 限流

## 请求过滤

## 请求重写

## 分布式