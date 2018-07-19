---
layout: post
title: AngularJs的学习
date: 2017-11-15 21:28:10
tags: AngularJs
---
##Controller使用过程照片中的注意点
不要试图去复用Controller，一个控制器一般只负责一小块视图
不要在Controller中操作DOM，这不是控制器的职责
不要在Controller里面做数据格式化，ng有很好的表单控件
不要在Controller里面做数据过滤操作，ng有$filter服务
一般来说，Controller是不会互相调用的，控制器之间的交互会通过事件进行

##神奇的$scope
$scope是一个POJO（Plain Old Javascript Object）
$scope提供了一些工具方法$watch()/apply()
$scope是一个树型结构，与DOM标签平行
$scope是表达式的执行环境（或者叫作用域）
子$scope对象会继承父$scope上的属性和方法
每一个Angular应用只有一个根$scope对象（一般位于ng-app上）
$scope可以传播事件，类似DOM事件，可以向上也可以向下
$scope不仅是MVC的基础，也是后面实现双向数据绑定的基础
可以用angular.element($0).scope进行调试