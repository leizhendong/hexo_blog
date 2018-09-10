---
title: ssh-keygen 基本用法
date: 2018-09-10 10:58:37
tags:
  - Linux
  - SSH
categories:
  - Linux
---

ssh 公钥认证是ssh认证的方式之一。通过公钥认证可实现ssh免密码登陆，git的ssh方式也是通过公钥进行认证的。

在用户目录的home目录下，有一个.ssh的目录，和当前用户ssh配置认证相关的文件，几乎都在这个目录下。

ssh-keygen 可用来生成ssh公钥认证所需的公钥和私钥文件。
```
使用 ssh-keygen 时，请先进入到 ~/.ssh 目录，不存在的话，请先创建。
并且保证 ~/.ssh 以及所有父目录的权限不能大于 711
```

## 生成ssh key
### 文件名称
生成ssh key的时候，可以通过 -f 选项指定生成文件的文件名，如下:
```
$ ssh-keygen -f test    -C "test key"
           -f 文件名(注意路径)   -C 备注
```
生成的文件在当前目录下,可以指定到固定路径下.

如果不使用参数指定文件名，会询问你输入文件名: 默认为 `id_rsa`
```
$ ssh-keygen
Generating public/private rsa key pair.
Enter file in which to save the key (/home/huqiu/.ssh/id_rsa):
你可以输入你想要的文件名，这里我们输入test。
```
### 密码 (不需要可直接回车)
之后，会询问你是否需要输入密码。输入密码之后，以后每次都要输入密码。请根据你的安全需要决定是否需要密码，如果不需要，直接回车:
```
$ ssh-keygen -t rsa -f test -C "test key"
Generating public/private rsa key pair.
Enter passphrase (empty for no passphrase):
Enter same passphrase again:
```

## 文件的权限
为了让私钥文件和公钥文件能够在认证中起作用，请确保权限正确。

对于.ssh 以及父文件夹，当前用户用户一定要有执行权限，其他用户最多只能有执行权限。

对于公钥和私钥文件也是: 当前用户一定要有执行权限，其他用户最多只能有执行权限。

```
对于利用公钥登录，对其他用户配置执行权限是没有问题的。
但是对于git，公钥和私钥, 以及config等相关文件的权限，其他用户不可有任何权限。
```

## 相关使用
1.拿到自己的公钥 本地生成的 `.ssh/id_rsa.pub` 文件.
2.写入到服务器用户目录下 `.ssh/authorized_keys` 文件.

3.注意文件的权限
```
chmod 700 .ssh
chmod 600  authorized_keys
```
4.重启服务
```
service sshd restart
或
systemctl restart sshd.service 
```