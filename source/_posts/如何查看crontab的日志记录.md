---
title: 如何查看crontab的日志记录
date: 2018-09-10 14:18:17
tags:
  - Linux
  - crontab
categories:
  - Linux
---
在Unix和类Unix的操作系统之中，crontab命令常用于设置周期性被执行的指令，也可以理解为设置定时任务。

crontab中的定时任务有时候没有成功执行，什么原因呢？这时就需要去日志里去分析一下了，那该如何查看crontab的日志记录呢？

## 1. linux
看 /var/log/cron.log这个文件就可以，可以用tail -f /var/log/cron.log观察

## 2. unix
在 /var/spool/cron/tmp文件中，有croutXXX001864的tmp文件，tail 这些文件就可以看到正在执行的任务了。

## 3. mail任务
在 /var/spool/mail/root 文件中，有crontab执行日志的记录，用tail -f /var/spool/mail/root 即可查看最近的crontab执行情况。


有朋友问到关于linux的crontab不知道是否到底执行了没有，也算写过一些基本备份的shell脚本，结合自己的实际生产环境简单讲述下如何通过cron执行的日志来分析crontab是否正确执行。

例如服务器下oracle用户有如下的计划任务
[oracle@localhost6 ~]$ crontab -l
00 1 * * 0 /home/oracle/backup/hollyipcc.sh
00 1 1 * * /home/oracle/backup/hollyreport_hollycrm.sh

关于系统的计划任务都会先在/var/log
[root@localhost ~]# cd /var/log/
[root@localhost log]# less cron
Sep 22 04:22:01 localhost crond[32556]: (root) CMD (run-parts /etc/cron.weekly)
Sep 22 04:22:01 localhost anacron[32560]: Updated timestamp for job `cron.weekly' to 2013-09-22
Sep 22 05:01:01 localhost crond[22768]: (root) CMD (run-parts /etc/cron.hourly)
Sep 22 06:01:01 localhost crond[25522]: (root) CMD (run-parts /etc/cron.hourly)
Sep 22 07:01:01 localhost crond[28255]: (root) CMD (run-parts /etc/cron.hourly)
Sep 22 08:01:01 localhost crond[30982]: (root) CMD (run-parts /etc/cron.hourly)
。。。

上面的/var/log/cron只会记录是否执行了某些计划的脚本，但是具体执行是否正确以及脚本执行过程中的一些信息则linux会每次都发邮件到该用户下。

