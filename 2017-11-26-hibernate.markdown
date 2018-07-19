---
layout: post
title: hibernate 数据库方言启动报错
date: 2017-11-26 22:49:16
tags:
---
hibernate启动报错，报错信息为：Error executing DDL via JDBC Statement
我使用的数据库版本是5.7.19，所以将方言设置为org.hibernate.dialect.MySQL5InnoDBDialect即可。