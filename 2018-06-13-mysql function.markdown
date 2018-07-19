---
layout: post
title: mysql遇到的函数
date: 2018-06-13 00:00:00
tags: mysql
---

### GROUP_CONCAT
详细：[mysql之group_concat函数详解][1]{:target="_blank"}

group_concat( [DISTINCT]  要连接的字段   [Order BY 排序字段 ASC/DESC]   [Separator '分隔符'] ),默认分隔符是逗号，对字段进行拼接。

```
select REACT_PID,group_concat(REACT_ID) from react GROUP BY REACT_PID;
select group_concat(REACT_ID) from react;
```

### LENGTH
返回字符串所占的字节数，计算字段的长度一个汉字是算三个字节,一个数字或字母算一个字节。

```
content | LENGTH(content)
没有    | 6
```

### CHAR_LENGTH、CHARACTER_LENGTH
返回字符串的长度，长度的单位为字符。多字节汉字、数字、字母都算一个字符。

```
content | CHAR_LENGTH(content)
没有    | 2
```

### BIT_LENGTH
返回二进制长度

```
content | BIT_LENGTH(content)
48      | 没有
```

### FIND_IN_SET
详细：[mysql中find_in_set()函数的使用][2]{:target="_blank"}

FIND_IN_SET(str,strlist)，str为要查询的字符串，strlist 字段名 参数以”,”分隔
查询字段(strlist)中包含(str)的结果，返回结果为null或记录

```
select FIND_IN_SET('1', '1');
SELECT id,name from sys_user WHERE FIND_IN_SET('vv',name);//name值类似'1,2,3,vv'
```

  [1]: https://www.cnblogs.com/wenxinphp/p/5841430.html
  [2]: https://www.cnblogs.com/xiaoxi/p/5889486.html