---
layout: post
title: 修改maven的默认JDK版本
date: 2018-05-29 00:00:00
tags: maven
---

maven项目默认JDK是1.5版本的，1.5的JDK中注解@override不支持对接口方法的实现，每次update project都会变回1.5的JDK，故项目报了很多红叉。当然对mvn clean和install是没有影响的。

参考链接：[如何修改maven的默认jdk版本][1]

### 解决办法一
在项目中的pom.xml指定jdk版本,只针对当前项目

```
<build>
    <plugins>
        <plugin>
            <groupId>org.apache.maven.plugins</groupId>
            <artifactId>maven-compiler-plugin</artifactId>
            <version>3.1</version>
            <configuration>
                <source>1.8</source>
                <target>1.8</target>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### 解决办法二
修改maven的settings.xml文件，添加配置如下：

```
<profile>    
    <id>jdk-1.8</id>    
    <activation>    
        <activeByDefault>true</activeByDefault>    
        <jdk>1.8</jdk>    
    </activation>    
    <properties>    
        <maven.compiler.source>1.8</maven.compiler.source>    
        <maven.compiler.target>1.8</maven.compiler.target>    
        <maven.compiler.compilerVersion>1.8</maven.compiler.compilerVersion>    
    </properties>    
</profile>  
```

该配置生效后，针对于所有maven项目都发挥作用

  [1]: https://www.cnblogs.com/Hxinguan/p/6132446.html