---
title: DBeaver等数据库软件无法加载身份插件的问题
date: 2020-12-29 19:07:36
tags: ["DBeaver"]
categories: ["数据库"]
---

## 连接mysql 出现：java.sql.SQLException: Unable to load authentication plugin ‘caching_sha2_password‘.

报错Exception during pool initialization.

java.sql.SQLException: Unable to load authentication plugin 'caching_sha2_password'.

报错与数据库有关的，应该是从MySQL 8.0.4开始, 默认的认证插件从mysql_native_password 变为caching_sha2_password. 

参考https://dev.mysql.com/doc/refman/8.0/en/caching-sha2-pluggable-authentication.html，创建新用户和密码

参考sql：
 CREATE USER 'yourusername'@'localhost'IDENTIFIED WITH mysql_native_password BY 'youpassword';
