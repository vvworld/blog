---
layout: post
title: 装系统出现error cannot load file code 5555h该如何解决
date: 2018-04-10 00:00:00
tags: 系统安装
---
参考链接：[链接][1]{:target="_blank"}

安装系统时，出现了error cannot load file code 5555h，根据参考链接的如下：

```
5555h分析：出现这个错误的根本原因在于，一般的Ghost XP系统安装盘无法重建MBR（也就是主引导记录），使得安装盘在一些隐藏分区的硬盘上无法加载文件，跟硬盘模式和分区类型没有关系的。
出错也没关系，咱们有强大的通用pe工具箱在！只要还能进bios系统，我们就能进pe系统，从而可以用pe系统下的工具来修复它。
```

然后百度了一下PE工具箱，搜索到 **U深度新一代U盘pe装系统工具**

ushedu官网上都有详细的教程，按照步骤安装即可。

[u深度u盘启动盘制作教程][2]{:target="_blank"}

[u深度u盘安装win7系统教程][3]{:target="_blank"}

  [1]: http://www.tongyongpe.com/n/201409/472.html
  [2]: http://www.ushendu.cn/jiaocheng/upqdzz.html
  [3]: http://www.ushendu.cn/jiaocheng/upzwin7.html