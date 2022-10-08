---
layout: post
title: 使用acme.sh给Nginx申请并配置HTTPS证书
keywords: acme
description: acme使用
tags: [ acme ]
---


## 1. 安装并配置acme
```bash
curl https://get.acme.sh | sh
source ~/.bashrc
```

## 2. 申请证书

- web root模式
```bash
# acme需要对/home/wwwroot/example.com有写权限
acme.sh  --issue  -d example.com  -w /home/wwwroot/example.com
```
- nginx模式
```bash
acme.sh  --issue  -d example.com  --nginx

# 指定Nginx配置文件
acme.sh  --issue  -d example.com  --nginx /etc/nginx/nginx.conf

# 指定站点
acme.sh  --issue  -d example.com  --nginx /etc/nginx/conf.d/example.com.conf
```

其他模式请看官方文档 [How to issue a cert ?](https://github.com/acmesh-official/acme.sh/wiki/How-to-issue-a-cert)

## 3. 安装证书
```bash
acme.sh --installcert -d example.com \
    --keypath /home/ubuntu/www/ssl/example.com.key  \
    --fullchainpath /home/ubuntu/www/ssl/example.com.pem \
    --reloadcmd "sudo service nginx force-reload/restart"
```

生成 dhparam.pem 文件
```bash
openssl dhparam -out /home/ubuntu/www/ssl/dhparam.pem 2048
```

## 4. Nginx配置SSL
```bash
http {
  # 新增
  ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
  ssl_prefer_server_ciphers on;
  # 兼容其他老浏览器的 ssl_ciphers 设置请访问 https://wiki.mozilla.org/Security/Server_Side_TLS

  server {
    listen 80 default_server;
    # 新增
    listen 443 ssl;
    ssl_certificate         /home/ubuntu/www/ssl/example.com.pem;
    ssl_certificate_key     /home/ubuntu/www/ssl/example.com.key;
    # ssl_dhparam 
    ssl_dhparam             /home/ubuntu/www/ssl/dhparam.pem;

    # 其他省略
  }
}
```
检查 Nginx 配置是否正确后重启
```bash
sudo service nginx configtest
sudo service nginx restart
```

## 5. crontab
```bash
acme.sh --cron

acme.sh --cron -f
```

## 附
[Caddy](https://caddyserver.com/docs/quick-starts/https)

[How to issue a cert](https://github.com/acmesh-official/acme.sh/wiki/How-to-issue-a-cert)

[部署使用 acme.sh 给 Nginx 安装 Let’ s Encrypt 提供的免费 SSL 证书](https://ruby-china.org/topics/31983)

[使用acme.sh给Nginx配置HTTPS证书](https://icode.best/i/87436139979028)

https://www.jianshu.com/p/7c0bb5becb85

https://www.cxyzjd.com/article/aouoy/115748036
