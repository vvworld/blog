---
layout: post
title: linux上vim中文乱码
date: 2018-07-18 00:00:00
tags: linux vim
---
查阅资料，编辑/etc/vimrc文件,添加以下几行即可解决中文乱码问题。
 
```
set fileencodings=utf-8,ucs-bom,gb18030,gbk,gb2312,cp936
set termencoding=utf-8
set encoding=utf-8
```