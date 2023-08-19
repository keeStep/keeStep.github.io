---
layout: post
title: 'GitHub克隆与认证的那些事copy'
date: 2023-08-15
author: Kee
tags: Git
---

# 第一种方式：SSH方式
## 克隆方式：`使用git@开头的链接`
## 认证方式：`需要公钥结合私钥进行认证`

> 1.生成SSH公私密钥key：
``` shell
ssh-keygen -t rsa -C "xxxx@163.com"

# -C 是你的github邮箱账号
```
 默认私钥文件名称：id_rsa </br>
 默认公钥文件名称：id_rsa.pub

> 2.GitHub配置公钥
``` shell
# 位置
点击头像 -> settings -> SSH and GPG keys
```
> [!NOTE]\
> 3.复制目标仓库的SSH链接进行克隆
``` shell
git clone git@github.com:keeStep/mybatis-keestep.git
```
---

# 第二种方式：HTTPS方式
## 克隆方式：`使用https开头的链接`
## 认证方式：`username/password`

> [!WARNING]\
> github改革后不能直接使用github登录密码进行认证了，需要从github获取token当做密码输入

> 1.获取token
``` shell
# 位置
点击头像 -> settings.Developer -> Settings.Personal access tokens (classic) 
```

> 2.复制目标仓库的HTTPS链接进行克隆
``` shell
git clone git@github.com:keeStep/mybatis-keestep.git
```

> 3.根据提示输入username和password
username：提交代码的名字即可，可以和git账号不一致
password：粘贴生成的token