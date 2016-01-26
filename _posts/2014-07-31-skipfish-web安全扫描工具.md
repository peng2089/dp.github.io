---
layout: post
title: skipfish – web安全扫描工具
keywords: skipfish,web安全,扫描,工具
description: Skipfish是由google出品的一款自动化的网络安全扫描工具，该工具可以安装在linux、freebsd、MacOS X系统和windows(cygwin).
tags: [ 安全,扫描,工具 ]
---

> Skipfish是由google出品的一款自动化的网络安全扫描工具，该工具可以安装在linux、freebsd、MacOS X系统和windows(cygwin).

## 主页

    https://code.google.com/p/skipfish

## 安装依赖

    Libidn pcre libssl

下载软件包，解压缩，在目录下

    $ Make

生成执行文件 skipfish


## 运行

    $ touch new_dict.wl #创建字典
    $ ./skipfish -o output_dir -S existing_dictionary.wl -W new_dict.wl  http://www.example.com/some/starting/path.txt

结果会生成在指定目录out_put_dir下的html文档

## 参数说明


    skipfish web application scanner - version 2.10b
    Usage: /home/admin/workspace/skipfish/skipfish [ options ... ] -W wordlist -o output_dir start_url [ start_url2 ... ]

    Authentication and access options:
    验证和访问选项：

      -A user:pass      - use specified HTTP authentication credentials
                          使用特定的http验证
      -F host=IP        - pretend that 'host' resolves to 'IP'
      -C name=val       - append a custom cookie to all requests
                          对所有请求添加一个自定的cookie
      -H name=val       - append a custom HTTP header to all requests
                          对所有请求添加一个自定的http请求头
      -b (i|f|p)        - use headers consistent with MSIE / Firefox / iPhone
                          伪装成IE/FIREFOX/IPHONE的浏览器
      -N                - do not accept any new cookies
                          不允许新的cookies
      --auth-form url   - form authentication URL
      --auth-user user  - form authentication user
      --auth-pass pass  - form authentication password
      --auth-verify-url -  URL for in-session detection

    Crawl scope options:

      -d max_depth     - maximum crawl tree depth (16)最大抓取深度
      -c max_child     - maximum children to index per node (512)最大抓取节点
      -x max_desc      - maximum descendants to index per branch (8192)每个索引分支抓取后代数
      -r r_limit       - max total number of requests to send (100000000)最大请求数量
      -p crawl%        - node and link crawl probability (100%) 节点连接抓取几率
      -q hex           - repeat probabilistic scan with given seed
      -I string        - only follow URLs matching 'string' URL必须匹配字符串
      -X string        - exclude URLs matching 'string' URL排除字符串
      -K string        - do not fuzz parameters named 'string'
      -D domain        - crawl cross-site links to another domain 跨域扫描
      -B domain        - trust, but do not crawl, another domain
      -Z               - do not descend into 5xx locations 5xx错误时不再抓取
      -O               - do not submit any forms 不尝试提交表单
      -P               - do not parse HTML, etc, to find new links 不解析HTML查找连接

    Reporting options:

      -o dir          - write output to specified directory (required)
      -M              - log warnings about mixed content / non-SSL passwords
      -E              - log all HTTP/1.0 / HTTP/1.1 caching intent mismatches
      -U              - log all external URLs and e-mails seen
      -Q              - completely suppress duplicate nodes in reports
      -u              - be quiet, disable realtime progress stats
      -v              - enable runtime logging (to stderr)

    Dictionary management options:

      -W wordlist     - use a specified read-write wordlist (required)
      -S wordlist     - load a supplemental read-only wordlist
      -L              - do not auto-learn new keywords for the site
      -Y              - do not fuzz extensions in directory brute-force
      -R age          - purge words hit more than 'age' scans ago
      -T name=val     - add new form auto-fill rule
      -G max_guess    - maximum number of keyword guesses to keep (256)

      -z sigfile      - load signatures from this file

    Performance settings:

      -g max_conn     - max simultaneous TCP connections, global (40) 最大全局TCP链接
      -m host_conn    - max simultaneous connections, per target IP (10) 最大链接/目标IP
      -f max_fail     - max number of consecutive HTTP errors (100) 最大http错误
      -t req_tmout    - total request response timeout (20 s) 请求超时时间
      -w rw_tmout     - individual network I/O timeout (10 s) 
      -i idle_tmout   - timeout on idle HTTP connections (10 s)
      -s s_limit      - response size limit (400000 B) 限制大小
      -e              - do not keep binary responses for reporting 不报告二进制响应

    Other settings:

      -l max_req      - max requests per second (0.000000)
      -k duration     - stop scanning after the given duration h:m:s
      --config file   - load the specified configuration file

    Send comments and complaints to >heinenn@google.com<.

