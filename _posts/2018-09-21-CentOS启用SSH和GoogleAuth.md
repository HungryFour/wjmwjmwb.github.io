---
layout: post
title:  "有锁，跑的更安全"
date:   2018-09-21 22:34:30 +0800
tags: CentOS SSH GoogleAuth 二步验证
color: RGBA(150, 96, 240, 0.3)
cover: '../assets/source/safelock.jpeg'
subtitle: 'CentOS启用SSH和GoogleAuth二步验证'
--- 

#***CentOS启用SSH和Google-Auth***
## **SSH-Key**
### 生成ssh-key，并且修改文件权限
```js
$ssh-keygen
$chmod 700 /home/dev/.ssh
$chmod 644 /home/dev/.ssh/xxxxx
```
将生成的xxxxx.pub copy到本地
### 修改sshd_config
```js
$vim /etc/ssh/sshd_config
```
```js
RSAAuthentication yes # 启用RSA认证
PubkeyAuthentication yes # 启用公钥认证
AuthenticationMethods publickey
PasswordAuthentication no  #停止密码验证
```
### 重启sshd服务
``` js
$service sshd restart
```

## **google-authenticator** 
###### 同时推荐大家使用自家解锁产品 [MinaOTP](https://github.com/MinaOTP) 
### 安装google-authenticator
```js
$yum install -y  autoconf automake libtool pam-devel git qrencode
$git clone https://github.com/google/google-authenticator-libpam.git
$cd google-authenticator-libpam/
$./bootstrap.sh
$./configure
$make
$make install
$ln -s /usr/local/lib/security/pam_google_authenticator.so /usr/lib64/security/pam_google_authenticator.so
```

### 配置openssh
```js
$vim /etc/pam.d/sshd
```
```js
auth    required    pam_google_authenticator.so nullok
```
### 编辑/etc/ssh/sshd_config
```js
UsePAM yes
UseDNS no
ChallengeResponseAuthentication yes
# 针对不同的用户组，做不同的验权处理
Match Group devuser
        AuthenticationMethods publickey,keyboard-interactive
Match Group product
        AuthenticationMethods publickey
```
### 重启sshd
```js
$service sshd restart
```
### 生成google-auth
```js
$google-authenticator
```
### 编辑 /etc/pam.d/sshd
```js
$vim /etc/pam.d/sshd
```
```js
auth      required    pam_sepermit.so
auth      required    pam_google_authenticator.so
#auth       substack     password-auth
#auth       include      postlogin
account    required    pam_nologin.so
account    include      password-auth
password  include      password-auth
# pam_selinux.so close should be the first session rule
session    required    pam_selinux.so close
session    required    pam_loginuid.so
# pam_selinux.so open should only be followed by sessions to be executed in the user context
session    required    pam_selinux.so open env_params
session    optional    pam_keyinit.so force revoke
session    include      password-auth
session    include      postlogin
```
### 编辑 /etc/pam.d/su
```js
$vim /etc/pam.d/su
```
```js
auth            sufficient      pam_rootok.so
auth      required    pam_google_authenticator.so
# Uncomment the following line to implicitly trust users in the "wheel" group.
#auth          sufficient      pam_wheel.so trust use_uid
# Uncomment the following line to require a user to be in the "wheel" group.
#auth          required        pam_wheel.so use_uid
auth            substack        system-auth
auth            include        postlogin
account        sufficient      pam_succeed_if.so uid = 0 use_uid quiet
account        include        system-auth
password        include        system-auth
session        include        system-auth
session        include        postlogin
session        optional        pam_xauth.so
```
