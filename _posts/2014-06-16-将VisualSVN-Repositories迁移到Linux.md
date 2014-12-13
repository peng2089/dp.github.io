---
layout: post
title: 将VisualSVN Repositories迁移到Linux
keywords: svn,VisualSVN,迁移,linux
description: 将VisualSVN Repositories从Windows迁移到Linux(CentOS)
tags: [ svn,服务器,CentOS ]
---

>说明

> windows : 192.168.1.5

> linux   : 192.168.1.4


## Windows端

- 导出本地Repositories

        > svnadmin.exe dump e:/repositories/repo_name &gt; repo_name.dump


- 上传authz,htpasswd

把e:/repositories/下的authz上传到linux端的/data/svn下

把e:/repositories/下的htpasswd重命名为passwd上传到linux端的/data/svn下,并修改其内容为

```
[users]
test = 123456
```

## Linux端

- 创建库(Linux端)

        # svnadmin create /data/svn/repo_name


- 导入dump文件

        # svnadmin load /data/svn/repo_name &lt; repo_name.dump

- 修改/data/svn/svnserve.conf

```
    [general]
    password-db = /data/svn/passwd
    authz-db = /data/svn/authz
```

- 启动svn服务

        # svnserver -d -r /data/svn


- 访问

        # svn checkout svn://192.168.1.4/repo_name