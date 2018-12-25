---
layout: post
title: "Install nginx using compile mode and configure ssl encrypted proxy service"
description: "编译安装Nginx并配置ssl加密的代理服务"
categories: [Web-Service]
tags: [SSL, nginx, https, pcre, openssl]
redirect_from:
  - /2018/04/02/
---

* Kramdown table of contents
{:toc .toc}

编译安装nginx并配置ssl加密的代理服务
==========

# 前期准备
安装编译需要的gcc和gcc-c++
```
yum install -y gcc gcc-c++
```
安装nginx依赖pcre-devel、openssl-devel、zlib-devel
```
yum install -y pcre pcre-devel openssl openssl-devel zlib zlib-devel
```
准备源码包并解压：
openssl-1.0.2o.tar.gz
```
tar -zxvf openssl-1.0.2o.tar.gz
```
pcre-8.41.tar.gz
```
tar -zxvf pcre-8.41.tar.gz
```
nginx-1.12.2.tar.gz
```
tar -zxvf nginx-1.12.2.tar.gz
```

# 安装依赖和nginx
```
cd ~/nginx-1.12.2/
./configure --prefix=~/app/nginx --with-pcre=~/pcre-8.41 --with-
http_stub_status_module --with-http_ssl_module --with-openssl=~/openssl-1.0.2o
make && make install
```
# 生成密钥文件
```
openssl gensa -out ca.key 2048
openssl genrsa -out ca.key 2048
openssl req -new -key ca.key -out ca.csr
openssl x509 -req -days 3650 -in ca.csr -signkey ca.key -out ca.crt
```

# 配置代理
```
    server {
        listen       8080 ssl;
        server_name  localhost;
        ssl_certificate ~/ca.crt;
        ssl_certificate_key ~/ca.key;
        ssl_session_timeout 60m;
        access_log  logs/10119032.access.log;
        location / {
            proxy_pass http://ip:port/;
            proxy_redirect default ;
        }
    }
```

# 参考
* [https://hostpresto.com/community/tutorials/how-to-install-the-apache-web-server-with-ssl-support-on-centos-7/](https://hostpresto.com/community/tutorials/how-to-install-the-apache-web-server-with-ssl-support-on-centos-7/)
