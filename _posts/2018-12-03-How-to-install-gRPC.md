---
layout: post
title: 如何安装gRPC
keywords: gRPC
description: 如何安装gRPC
tags: [ gRPC,Go ]
---

# 如何安装gRPC

## 问题描述

```
package google.golang.org/grpc: unrecognized import path "google.golang.org/grpc"(https fetch: Get https://google.golang.org/grpc?go-get=1: dial tcp 216.239.37.1:443: i/o timeout)
```

原因是这个代码已经转移到github上面了，但是代码里面的包依赖还是没有修改，还是google.golang.org这种，



## 安装

```bash
$ git clone https://github.com/grpc/grpc-go.git $GOPATH/src/google.golang.org/grpc
$ git clone https://github.com/golang/net.git $GOPATH/src/golang.org/x/net
$ git clone https://github.com/golang/text.git $GOPATH/src/golang.org/x/text
$ git clone https://github.com/golang/sys.git $GOPATH/src/golang.org/x/sys
$ go get -u github.com/golang/protobuf/{proto,protoc-gen-go}
$ git clone https://github.com/google/go-genproto.git $GOPATH/src/google.golang.org/genproto
$ cd $GOPATH/src/
$ go install google.golang.org/grpc
```