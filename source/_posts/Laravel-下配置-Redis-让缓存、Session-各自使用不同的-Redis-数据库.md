---
title: Laravel 下配置 Redis 让缓存、Session 各自使用不同的 Redis 数据库
date: 2018-09-10 11:47:54
tags:
  - Laravel
  - PHP
categories:
  - Laravel
---

默认情况下，Redis 服务会提供 16 个数据库，Laravel 使用数据库 0 （请见 Redis 文档）作为缓存和 Session 的存储。

在执行命令 php artisan cache:clear 清除缓存时，会把 Session 也连带清除了，可以通过以下设置来避免这个问题。

## 开始配置
我们的目的是让缓存，也就是默认的 Redis 存储到 0 号数据库，Session 存储在 1 号数据库。

### 1. 配置 Session Redis 数据库
修改 config/database.php，在 redis 选项内增加 session 选项，并把 database 修改为 1：

```
'redis' => [

   'cluster' => false,

   'default' => [
       'host'     => env('REDIS_HOST', 'localhost'),
       'password' => env('REDIS_PASSWORD', null),
       'port'     => env('REDIS_PORT', 6379),
       'database' => 0,
   ],

   'session' => [
         'host'     => env('REDIS_HOST', 'localhost'),
         'password' => env('REDIS_PASSWORD', null),
         'port'     => env('REDIS_PORT', 6379),
         'database' => 1,
   ],
],
```
### 2. 指定 Session 使用数据库
修改 config/session.php ，把下面这一行：
```
'connection' => null,
```
改为：
```
'connection' => 'session',
```
### 3. 开始使用
修改 .env 文件的 SESSION_DRIVER 选项为 redis，开始应用上。
```
SESSION_DRIVER=redis
```
### 4. 测试一下
执行以下命令后检查下是否退出登录：
```
php artisan cache:clear
```


需要的redis包~
```
composer require predis/predis
```

相关参考:
redis 官网 https://redis.io/download

