---
layout: post
title: Go-MySQL-Driver使用说明
keywords: golang, sql, Go-MySQL-Driver
description: Go-MySQL-Driver使用说明
tags: [ go,sql]
---

# Go-MySQL-Driver
> A MySQL-Driver for Go's database/sql package

## 官网

[https://github.com/go-sql-driver/mysql] (https://github.com/go-sql-driver/mysql)

## 下载

```
go get github.com/Go-SQL-Driver/MySQL
```

## 导入

```
import (
	"database/sql"
	_"github.com/Go-SQL-Driver/MySQL"
)
```

## 链接数据库

```
db, err := sql.Open("mysql", "用户名:密码@tcp(IP:端口)/数据库?charset=utf8")
如: db, err = sql.Open("mysql", "root:@tcp(127.0.0.1:3306)/test?charset=utf8")
```
## 增删改查 

```sql
CREATE TABLE `tbl_user` (
  `id` int(11) NOT NULL,
  `username` varchar(16) NOT NULL COMMENT '用户名',
  `password` char(32) NOT NULL COMMENT '密码'
) ENGINE=InnoDB DEFAULT CHARSET=latin1 COMMENT='用户表';
```

1. insert

```
# 1. 直接使用Exec函数添加
res, err := db.Exec("INSERT INTO tb_user (username, password) VALUES (?, ?)","test","123456")
if err != nil {
	log.Fatal(err)
}
# 2. 首先使用Prepare获得stmt，然后调用Exec添加
stmt, err := db.Prepare("INSERT tb_user SET username=?,password=?")
if err != nil {
	log.Fatal(err)
}
res, err := stmt.Exec("test", "123456")
if err != nil {
	log.Fatal(err)
}
# 获取刚刚插入的记录的id
id, err := res.LastInsertId()
if err != nil {
	log.Fatal(err)
}
```

2. update

```golang
stmt, err := db.Prepare("UPDATE `tb_user` SET `password`=? WHERE `username`=?")
if err != nil {
	log.Fatal(err)
}
res, err := stmt.Exec("654321", "test")
if err != nil {
	log.Fatal(err)
}
num, err := res.RowsAffected()
if err != nil {
	log.Fatal(err)
}
```

3. select

```golang
# 1. 查询单条数据
var id int
var username, password string

err := db.QueryRow("SELECT `id`, `username`, `password` FROM `tb_user` WHERE `username`=?", "test").Scan(&id, &username, &password)
if err != nil {
	log.Fatal(err)
}

# 2. 查询多条数据，并遍历
rows, err := db.Query("SELECT `username`, `password` FROM `tbl_user` WHERE `username`=?", "test")
if err != nil {
	log.Fatal(err)
}
for rows.Next() {
	var id int
	var username, password string
	if err := rows.Scan(&id, &username, &password); err == nil {
		log.Fatal(err)
	}
	fmt.Println(id)
	fmt.Println(username)
	fmt.Println(password)
}
```

4. delete

```golang
stmt, err := db.Prepare("DELETE FROM `tb_user` WHERE `username`=?")
if err != nil {
	log.Fatal(err)
}
res, err := stmt.Exec("test")
if err != nil {
	log.Fatal(err)
}
num, err := res.RowsAffected()
if err != nil {
	log.Fatal(err)
}
```

## 事务

```golang
t, err := db.Begin()
if err != nil {
	log.Fatal(err)
}
stmt, err1 := t.Prepare("INSERT INTO `tb_user` (`username`, `password`) VALUES (?, ?)");
if err1 != nil {
	log.Fatal(err1)
}
_, err2 := stmt.Exec("test", "123456")

if err2 != nil {
	log.Fatal(err2)
}

err3 := t.Commit()
if err3 != nil {
	err4 := t.Rollback()
	if err4 != nil {
		log.Fatal(err4)
	}
}
```



