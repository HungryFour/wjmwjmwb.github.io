---
layout: post
title:  "翻出去，看看生活的美好"
date:   2018-09-20 21:34:30 +0800
tags: Shadowsocks 
color: RGBA(150, 96, 240, 0.3)
cover: '../assets/source/fanchuqu.jpeg'
subtitle: 'CentOS7下搭建Shadowsocks'
--- 

### 使用pip安装shadowsocks
```js
$pip install --upgrade pip  # 升级pip
$pip install shadowsocks   # 安装
```
### 创建配置文件/etc/shadowsocks.json
```js
$vim /etc/shadowsocks.json
```
```js
{
  "server": "0.0.0.0",
  # "server_port": 8388,  # 单端口（账号）模式/端口随意设置
  # "password": "XXXXXXXXXXXXXX",  #单端口（账号）模式/密码
  "port_password":{  # 多端口（账号）模式
    "8380": “XXXXXXXXX",
    "8381": “XXXXXXXXXX"
 },
  "method": "aes-256-cfb"  # aes-128-cfb, aes-192-cfb, aes-256-cfb, bf-cfb, cast5-cfb, des-cfb, rc4-md5, chacha20, salsa20, rc4, table
}
```
### 配置启动脚本文件/etc/systemd/system/shadowsocks.service
```js
$vim /etc/systemd/system/shadowsocks.service
```
```js
[Unit]
Description=Shadowsocks

[Service]
TimeoutStartSec=0
ExecStart=/usr/bin/ssserver -c /etc/shadowsocks.json

[Install]
WantedBy=multi-user.target
```
### 启动shadowsocks服务
```js
$systemctl enable shadowsocks
$systemctl start shadowsocks
```
### 查看shadowsocks服务状态
```js
$systemctl status shadowsocks -l
```
#### 成功结果类似：
```js
● shadowsocks.service - Shadowsocks
   Loaded: loaded (/etc/systemd/system/shadowsocks.service; enabled; vendor preset: disabled)
   Active: active (running) since Thu 2018-07-12 05:30:39 UTC; 19s ago
 Main PID: 20530 (ssserver)
   CGroup: /system.slice/shadowsocks.service
           └─20530 /usr/bin/python /usr/bin/ssserver -c /etc/shadowsocks.json

Jul 12 05:30:39 vultr.guest systemd[1]: Started Shadowsocks.
Jul 12 05:30:39 vultr.guest systemd[1]: Starting Shadowsocks...
Jul 12 05:30:39 vultr.guest ssserver[20530]: INFO: loading config from /etc/shadowsocks.json
Jul 12 05:30:39 vultr.guest ssserver[20530]: 2018-07-12 05:30:39 INFO     loading libcrypto from libcrypto.so.10
Jul 12 05:30:39 vultr.guest ssserver[20530]: 2018-07-12 05:30:39 INFO     starting server at 0.0.0.0:8381
Jul 12 05:30:39 vultr.guest ssserver[20530]: 2018-07-12 05:30:39 INFO     starting server at 0.0.0.0:8380

```
### 因为CentOS 7防火墙默认阻止ss端口，所以需要开放上述安装时设置的端口
```js
$firewall-cmd --permanent --add-port=8380/tcp
$firewall-cmd --permanent --add-port=8381/tcp
...
$firewall-cmd --reload
```