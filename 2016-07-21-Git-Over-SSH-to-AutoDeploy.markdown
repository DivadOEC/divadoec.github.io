---
layout: post
title:  "Git Over SSH to AutoDeploy"
date:   2016-07-21 17:14:40 +0800
categories: jekyll update
tags:	[Git,SSH,AutoDeploy]
---

# 部署机安装与配置
## 安装软件
### 安装git（root用户）
上传git源代码并编译安装：

```
tar xvzf git-2.6.4.tar.gz
cd git-2.6.4
./configure
make
make install
```

验证： ```git --version``` 返回版本号2.6.4
 

### 安装svn 1.7+ （root用户，可选）
(SVN1.7以上不会在每个目录下建.svn子目录)

上传svn源代码并编译安装，需root用户：

```
tar xvzf subversion-1.7.4.tar.gz
cd subversion-1.7.4
cd neon
./configure
make
make install
cd ..
./configure
make
make install
```

验证：```svn --version``` 返回版本号1.7.4

## 配置SSH
为当前操作用户（deploy）生成密钥对

```
cd
mkdir .ssh
chmod 700 .ssh
cd .ssh
ssh-keygen
```

验证，在.ssh目录下有id_rsa与id_rsa.pub文件

## 配置git
设置用户名与邮箱

```
git config --global user.name "ccic-net"
git config --global user.email "user@ccic-net.com.cn"
```

编辑默认忽略列表: ~/.gitignore

```
.svn
.git
```

配置成GIT全局设置

```
git config --global core.Excludesfile "~/.gitignore"
```

## 初始化部署文件夹
部署目录放在 /ccicall/deploy/SYSTEM/APP 下。

```
mkdir -p /ccicall/deploy/SYSTEM
```

### 从svn中下载部署文件
(也可以采用手工上传部署文件的方式)

```
cd /ccicall/deploy/SYSTEM
svn co http://10.1.33.55:83/deploy/APP APP
```

### 初始化git库

```
cd /ccicall/deploy/SYSTEM/APP
git init
cd ..
git clone --bare APP APP.git
```

### 提交初始文件到本地git

```
cd /ccicall/deploy/SYSTEM/APP
git add *
git commit -m 'init'
```

# 受管服务器的配置
## 配置SSH
### 添加部署机公钥
为用户（如 ccicall、weblog10等 ）添加部署机公钥（将部署机的 .ssh/id_rsa.pub 内容复制到受管机的 .ssh/authorized_keys 中）

```
cd
chmod 755 .
mkdir .ssh
chmod 700 .ssh
cd .ssh
vi authorized_keys
chmod 600 authorized_keys
```

### 开启SSH证书认证模式（root用户）
编辑/etc/ssh/sshd_config

```
RSAAuthentication yes 
PubkeyAuthentication yes 
AuthorizedKeysFile %h/.ssh/authorized_keys
```

重启SSHD

```
service sshd restart
```

### 验证
通过部署机尝试连接受管服务器

```
ssh user@host
```

## 安装GIT软件（通过部署机操作）
### 安装git

```
scp git-2.6.4.tar.gz user@host:/tmp/
ssh user@host
su - root
cd /tmp
tar xvzf git-2.6.4.tar.gz
cd git-2.6.4
./configure
make
make install
cd /tmp
rm -fR git-2.6.4*
```

## 首次发布部署文件
通过部署机推送部署文件到受管服务器，假设受管服务器的部署目录为 /Prog/APP/

```
cd /ccicall/deploy/SYSTEM/
scp -r APP.git user@host:/Prog/
cd APP
git push user@host:/Prog/APP.git master
ssh user@host "cd /Prog/; git clone APP.git APP"
```

# 日常发布操作
在部署机上操作

## 获取更新
### 从SVN下载更新
(也可以采用手工上传部署文件的方式)

```
cd /ccicall/deploy/SYSTEM/APP
svn up
```

## 检查更新文件
```
cd /ccicall/deploy/SYSTEM/APP
git status
```

## 确认更新并提交本地git
```
git add xxx
git commit -m "yyyymmdd"
```


## 推送更新到受管机
```
git push user@host:/Prog/APP.git master
ssh user@host "cd /Prog/APP; git pull"
```

## 重启应用
```
ssh user@host "restart"
```


