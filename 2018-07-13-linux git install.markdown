---
layout: post
title: linux上git的安装
date: 2018-07-13 00:00:00
tags: linux git
---
### git安装依赖

```
sudo yum -y install zlib-devel openssl-devel cpio expat-devel gettext-devel curl-devel perl-ExtUtils-CBuilder perl-ExtUtils- MakeMaker 
```

上面可能会默认装上低版本的git，且用yum -y install git也是安装的低版本。

### git的卸载

```
sudo yum remove git
```

### 下载git软件包
可以通过此处查找到自己需要的git版本：[git版本][1]

```
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.9.5.tar.gz
```

### 解压软件包

```
tar -zxvf git-2.9.5.tar.gz
```

### 编译并安装

```
make prefix=/usr/local/git all
make prefix=/usr/local/git install
```

### 查找git的安装路径

```
whereis git
```

### 添加环境变量

```
vi /etc/profile
export PATH=$PATH:/usr/local/git/bin
source /etc/profile
```

### 查看git版本，验证其是否生效

```
git --version
```

### git系统配置
参考文档：[git如何使用SSH密钥][2]

```
[root@VM_16_8_centos git-2.9.5]# git config --global user.name "username"
[root@VM_16_8_centos git-2.9.5]# git config --global user.email "useremail"
[root@VM_16_8_centos git-2.9.5]# git config --global core.autocrlf false
[root@VM_16_8_centos git-2.9.5]# git config --global core.quotepath off  //支持utf-8编码
[root@VM_16_8_centos git-2.9.5]# git config --global gui.encoding utf-8  //gui编码方式
[root@VM_16_8_centos git-2.9.5]# ssh-keygen -t rsa -C "useremail"
[root@VM_16_8_centos git-2.9.5]# ssh-add ~/.ssh/id_rsa
Identity added: /root/.ssh/id_rsa (/root/.ssh/id_rsa)
[root@VM_16_8_centos git-2.9.5]# cat ~/.ssh/id_rsa.pub
```


  [1]: https://mirrors.edge.kernel.org/pub/software/scm/git/
  [2]: https://www.cnblogs.com/superGG1990/p/6844952.html