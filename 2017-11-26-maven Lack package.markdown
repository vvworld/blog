---
layout: post
title: maven缺包
date: 2017-11-14 15:11:51
tags: maven
---
Eclipse引入maven项目后，由于pom文件中jar包不存在，导致项目没有引进maven依赖包，从而导致代码报错。这时应先修改pom文件，使得相对应的包正确，然后(在**Navigator**视图下)项目右键**Maven**-->**Disable Maven Nature**,再**Configure**-->**Convert to Maven Project**，自然而然就引进了依赖包。