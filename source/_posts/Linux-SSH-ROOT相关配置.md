---
title: Linux SSH ROOT相关配置
date: 2018-09-10 10:28:41
tags:
  - Linux
  - SSH
categories:
  - Linux
---


## Linux修改ssh端口22

vi /etc/ssh/ssh_config

vi /etc/ssh/sshd_config

然后修改为port 8888


以root身份service sshd restart (redhat as3)

使用putty,端口8888

Linux下SSH默认的端口是22,为了安全考虑，现修改SSH的端口为1433,修改方法如下 ：

 /usr/sbin/sshd -p 1433

## 禁用root远程登录 

为增强安全 先增加一个普通权限的用户：

#useradd PGOne
//设置密码
#passwd PGOne

切换到root用户，运行visudo命令

在打开的配置文件中，找到root ALL=(ALL) ALL，在下面添加一行

xxx ALL=(ALL) ALL 其中xxx是你要加入的用户名称

 
### 生产机器禁止ROOT远程SSH登录：

#vi /etc/ssh/sshd_config

把

PermitRootLogin yes

改为

PermitRootLogin no


重启sshd服务

#service sshd restart