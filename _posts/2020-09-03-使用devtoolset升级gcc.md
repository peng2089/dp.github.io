---
layout: post
title: 使用devtoolset升级gcc
keywords: devtoolset, gcc
description: 使用devtoolset升级gcc
tags: [ devtoolset,gcc,centos,linux ]
---

安装devtoolset包
```bash
$ yum install centos-release-scl
$ yum install devtoolset-9
```

激活gcc版本，使其生效
```
$ scl enable devtoolset-9 bash
$ gcc --version # 查看版本
```

为了避免每次手动生效，可以在.bashrc中设置

```
$ source /opt/rh/devtoolset-9/enable
$ source scl_source enable devtoolset-9
```