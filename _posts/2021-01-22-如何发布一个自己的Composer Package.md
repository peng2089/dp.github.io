---
layout: post
title: 如何发布一个自己的Composer Package
keywords: composer
description: 如何发布一个自己的Composer Package
tags: [ composer ]
---
# 如何发布一个自己的Composer Package

## 前提条件
1. 本地安装了composer
2. 有一个github账号
3. 要有一个[https://packagist.org](https://packagist.org)的账号

## 1. 目录结构
```
| - composer.json
| - src             // 存放代码
| - README.md
```

## 2. 定义composer.json

如果熟悉composer.json可以手动创建一个, 如果不熟悉可以通过一下命令, 在命令的提示下一步步创建.
```
$ composer init
```
下面以guzzlehttp来说明文件中各个字段的意义
```
{
     "name": "guzzlehttp/guzzle",//composer 包名
     "type": "library",//这是默认类型，它会简单的将文件复制到vendor目录
     "description": "Guzzle is a PHP HTTP client library",//一个包的简短描述。通常这个最长只有一行。
     "keywords": ["framework", "http", "rest", "web service", "curl", "client", "HTTP client"],//在composer搜索用到
     "homepage": "http://guzzlephp.org/",//项目主页
     "license": "MIT",//包的许可协议
     "authors": [//作者
         {
             "name": "Michael Dowling",
             "email": "mtdowling@gmail.com",
             "homepage": "https://github.com/mtdowling"
         }
     ],
     "require": {//必要的依赖
         "php": ">=5.5",
         "guzzlehttp/psr7": "^1.4",
         "guzzlehttp/promises": "^1.0"
     },
     "require-dev": {//开发版的依赖，一般帮助开发阶段使用
             "ext-curl": "*",//可以帮你指定需要的 PHP 扩展（包括核心扩展）例如在编辑器的自动提示功能
             "phpunit/phpunit": "^4.8.35 || ^5.7 || ^6.4 || ^7.0",
             "psr/log": "^1.0"
     },
     "autoload": {//自动加载映射
             "files": ["src/functions_include.php"],//自动加载的函数库文件 相当于直接require该文件，直接使用其中function 数组可配置多个
             "psr-4": {
                "GuzzleHttp\\": "src/"
             }
     },
     "autoload-dev": {//一般将单元测试的路径放在这里
             "psr-4": {
                "GuzzleHttp\\Tests\\": "tests/"
             }
     },
     "suggest": {//建议安装的包
             "psr/log": "Required for using the Log middleware"
     },
     "extra": {//可选项
             "branch-alias": {
                "dev-master": "6.3-dev"
             }
     },
     "bin": [//composer安装时会将script脚本文件复制到vendor/bin中 比如 php vendor/bin/script 执行一些组件的脚本功能
         "bin/script"
     ]
}
```

## 3. 发布代码
1. 把代码发布到github上
2. 将github代码仓库的地址, 放到https://packagist.org/packages/submit check后提交, check的时候会提示name是不是重复了, 然后仓库里是不是有类似的代码
3. 提交后, 就可以看到你的package 在https://packagist.org的地址了

## END

[Composer官网](https://getcomposer.org/)
[Composer中文站](https://www.phpcomposer.com/)
[Composer 中文文档](https://learnku.com/docs/composer/2018)