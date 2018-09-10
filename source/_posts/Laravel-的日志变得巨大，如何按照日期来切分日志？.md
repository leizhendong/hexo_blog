---
title: Laravel 的日志变得巨大，如何按照日期来切分日志？
date: 2018-09-10 11:54:20
tags:
  - Laravel
  - PHP
categories:
  - Laravel
---
## 日志存储
Laravel 默认的错误文件记录在一个文件里，随着时间的推移，此文件将会变得巨大，不方便查阅。

我们可以通过修改 `config/app.php` 配置文件中的 `log` 选项来配置 `Laravel` 使用的存储机制。如果你希望每天产生日志都存放在不同的文件中，则应将 `app` 配置文件中的 `log` 值设置为 `daily`：
```
'log' => 'daily'
```

最大日志文件数
在使用 `daily` 日志模式时，`Laravel` 默认只保留五天份的日志文件。如果要调整保留文件的数量，就在 `app` 配置文件中添加一个 `log_max_files` 配置项：
```
'log_max_files' => 30
```