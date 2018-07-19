---
layout: post
title: Windows如何修改Mysql用户的root密码
date: 2017-10-31 22:12:12
tags: mysql
---
## cmd登录Mysql报错

```
mysql -u root -p 
Enter password:  
ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
```

## 解决方法
编辑mysql配置文件my.ini，在[mysqld]这个条目下加入 skip-grant-tables保存退出后重启mysql

停止： net stop mysql
启动： net start mysql

此时，cmd启动完mysql输入mysql -u root -p，此时mysql不需要登录密码。

```
mysql> use mysql;
mysql> update user set password=password("新密码") where user="root";
mysql> flush privileges;
```

## Unknown column 'password' in 'field list'
我的数据库版本是5.7的，该版本把password这个字段改成了authentication_string