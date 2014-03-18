# CentOS6.4配置163的yum源

- 下载repo文件
```
	[root@localhost ~]# wget http://mirrors.163.com/.help/CentOS6-Base-163.repo #6.*
	[root@localhost ~]# wget http://mirrors.163.com/.help/CentOS5-Base-163.repo #5.*
```

- 备份并替换系统的repo文件
```
	[root@localhost ~]# cd /etc/yum.repos.d/
	[root@localhost ~]# mv CentOS-Base.repo CentOS-Base.repo.bak
	[root@localhost ~]# mv CentOS6-Base-163.repo CentOS-Base.repo
```

- 执行yum源更新
```
	[root@localhost ~]# yum clean all
	[root@localhost ~]# yum makecache
	[root@localhost ~]# yum update
```