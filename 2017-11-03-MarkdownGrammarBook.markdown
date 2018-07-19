---
layout: post
title: Markdown语法手册
date: 2017-10-31 22:30:39
tags: markdown
---
参考链接：[欧薇娅-简书](https://www.jianshu.com/p/b03a8d7b1719)

# 目录
* [标题](#1)
* [分级标题](#2)
* [Toc](#3)
* [引用（段落）](#4)
* [行内标记](#5)
* [代码块](#6)
* [插入链接](#7)
* [插入图片](#8)
* [插入图片带有链接](#9)
* [粗体和斜体](#10)
* [序表](#11)

<h1 id='1'>1.标题</h1>
## 代码
注：#后面保持空格

    # h1
    ## h2
    ### h3
    #### h4
    ##### h5
    ###### h6
    ####### h7 //错误代码
    
## 演示
# h1
## h2
...
##### h5
###### h6
####### h7
<h1 id='2'>2.分级标题</h1>
## 代码
注：`=` `-`最少可以只写一个，兼容性一般

    一级标题
    =======
    二级标题
    --------

## 演示
一级标题
========
二级标题
--------
<h1 id='3'>3.Toc</h1>
注：根据标题生成目录，兼容性一般
## 代码

    [TOC]

<h1 id='4'>4.引用</h1>
## 代码1（单行式）

    > hello world!

## 演示
> hello world!

## 代码2（多行式）

    > hello world!
    hello world!
    hello world!

## 演示
> hello world!
hello world!
hello world!

或者

    > hello world!
    > hello world!
    > hello world!

## 演示
> hello world!
> hello world!
> hello world!

## 代码3（多层嵌套）

    > aaaaaaaaa
    >> bbbbbbbbb
    >>> cccccccccc

## 演示

> aaaaaaaaa
>> bbbbbbbbb
>>> cccccccccc

<h1 id='5'>5.行内标记</h1>
注：用 **`** 标记**代码块将变成一行**
## 代码

    标记之外`hello world`标记之外

## 演示

标记之外`hello world`标记之外

<h1 id='6'>6.代码块</h1>
## 代码1（Tab）
注：与上行距离一空行

    我是文字。。。
    
        <div>   
            <div></div>
            <div></div>
            <div></div>
        </div>

## 代码2（```）

    ```
    <div>   
        <div></div>
        <div></div>
        <div></div>
    </div>
    ```
    
## 演示

```
<div>   
    <div></div>
    <div></div>
    <div></div>
</div>
```

## 代码3（自定义语法）
注：根据不同的语言配置不同的代码着色

    ```javascript
    var num = 0;
    for (var i = 0; i < 5; i++) {
        num+=i;
    }
    console.log(num);
    ```

## 演示

```javascript
var num = 0;
for (var i = 0; i < 5; i++) {
    num+=i;
}
console.log(num);
```

<h1 id='7'>7.插入链接</h1>
## 代码1（内链式）
注：`{:target="_blank"}`跳转方式兼容性一般，多数第三方平台不支持跳转

    [百度1](http://www.baidu.com/" 百度一下"){:target="_blank"}

## 演示
[百度1](http://www.baidu.com/" 百度一下"){:target="_blank"}

## 代码2（引用式）
    
    [百度2][1]{:target="_blank"}
    [2]: http://www.baidu.com/   "百度二下"

[百度2][2]{:target="_blank"}
<h1 id='8'>8.插入图片</h1>
## 代码1（内链式）

    ![](http://vvworld.cn:9080/idol.jpg '文哥')

## 演示
![](http://vvworld.cn:9080/idol.jpg '文哥')
## 代码2（引用式）

    !['文哥'][1]
    [1]: http://vvworld.cn:9080/goddess.jpg

## 演示
!['文哥'][1]

<h1 id='9'>9.插入图片带有链接</h1>

    [![](http://vvworld.cn:9080/idol.jpg '文哥')](http://vvworld.cn:9080/idol.jpg){:target="_blank"}// 内链式
    [![](http://vvworld.cn:9080/idol.jpg '文哥')][2]{:target="_blank"}// 引用式
    [2]: http://vvworld.cn:9080/idol.jpg

## 演示

[![](http://vvworld.cn:9080/idol.jpg '文哥')](http://vvworld.cn:9080/idol.jpg){:target="_blank"}
[![](http://vvworld.cn:9080/idol.jpg '文哥')][2]{:target="_blank"}

  [2]: http://vvworld.cn:9080/idol.jpg
  [1]: http://vvworld.cn:9080/goddess.jpg
<h1 id='10'>10.粗体和斜体</h1>

    **粗体字** 正常字

## 演示
**粗体字**正常字

    *斜体字* 正常字

## 演示
*斜体字* 正常字
<h1 id='11'>11.序表</h1>
## 代码1（有序）
注：序列`.`后**保持空格**

    1. one
    2. two
    3. three

## 演示
1. one
2. two
3. three

## 代码2（无序）

    * one
    * two
    * three

## 演示
* one
* two
* three

## 代码3（序表嵌套）
    
    1. one
        1. one-1
        2. two-2
    2. two
        * one-1
        * two-2

## 演示
1. one
    1. one-1
    2. two-2
2. two
    * one-1
    * two-2