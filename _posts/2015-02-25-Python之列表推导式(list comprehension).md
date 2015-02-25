---
layout: post
title: Python 之 列表推导式(list comprehension)
keywords: 列表推导式,Python
description: 列表推导式（list comprehension）是利用其他列表创建新列表（类似于数学中的集合推导式）的一种方法。其工作方式类似于for循环.
tags: [ 开发,Python,列表推导式 ]
---


> 列表推导式（list comprehension）是利用其他列表创建新列表（类似于数学中的集合推导式）的一种方法。其工作方式类似于for循环

## 一般使用

循环从0开始的10个数字,且对每个数字平方

    >>> [x*x for x in range(10)]
    [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]

如果只想打印range(10)里面,只能被2整除的数字的平方

    >>> [x*x for x in range(10) if x % 2 == 0]
    [0, 4, 16, 36, 64]

## 替换循环嵌套

**常规嵌套**

    for x in [1,2,3]:
      for y in [1,2,3]:
        z = x*y
        print str(x)+'*'+str(y)+' is: '+str(z)

**列表推导式**

    print [x*y for x in [1,2,3] for y in  [1,2,3]]