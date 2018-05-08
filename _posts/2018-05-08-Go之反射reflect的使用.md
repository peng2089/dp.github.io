---
layout: post
title: Go之反射reflect的使用
keywords: golang,reflect,反射
description: Go之反射reflect的使用
tags: [ golang ]
---

> 反射的说明

1. 获取结构体的字段以及字段类型

```golang
package main

import (
	"reflect"
	"log"
	"fmt"
)
type Demo struct {
	Name string
	Age int
	Sex string
}
func main(){
	m := new(Demo)

	t := reflect.TypeOf(m)

	// 判断是否是指针
	if t.Kind() == reflect.Ptr {
		t = t.Elem() // 获取所指向的元素, 此处指的是结构体models.Media
	}
	// 判断m的类型是否是结构体
	if t.Kind() != reflect.Struct {
		log.Println("Check type error not Struct")
	}

	// 获取m的字段数目
	fieldNum := t.NumField()
	result := make(map[string]interface{})

	for i:= 0;i<fieldNum;i++ {
		// t.Field(i).Type: 获取字段类型
		// t.Field(i).Name: 获取字段名称
		result [t.Field(i).Name] = t.Field(i).Type
	}

	for k, v := range result {
		fmt.Printf("%s => %v \n", k, v)
	}
}
```

output:
```
Name => string 
Age => int 
Sex => string 
```