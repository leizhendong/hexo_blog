---
title: linux 防火墙 iptables 配置
date: 2018-09-10 10:53:55
tags:
  - Linux
  - iptables
categories:
  - Linux
---

## iptables 命令
1. 启动指令:service iptables start   
2. 重启指令:service iptables restart   
3. 关闭指令:service iptables stop 

```
systemctl restart iptables.service

```
```
相关配置:
vim /etc/sysconfig/iptables 

设置 iptables service

yum -y install iptables-services

如果要修改防火墙配置，如增加防火墙端口3306

vi /etc/sysconfig/iptables 

增加规则

-A INPUT -m state --state NEW -m tcp -p tcp --dport 3306 -j ACCEPT
```



### CentOS 7 以下版本 iptables 命令

如要开放80，22，8080 端口，输入以下命令即可
/sbin/iptables -I INPUT -p tcp --dport 80 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 22 -j ACCEPT
/sbin/iptables -I INPUT -p tcp --dport 8080 -j ACCEPT
然后保存：

/etc/rc.d/init.d/iptables save
查看打开的端口：

/etc/init.d/iptables status
关闭防火墙 
1） 永久性生效，重启后不会复原

开启： chkconfig iptables on

关闭： chkconfig iptables off

2） 即时生效，重启后复原

开启： service iptables start

关闭： service iptables stop

查看防火墙状态： service iptables status