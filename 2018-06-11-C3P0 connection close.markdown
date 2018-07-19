---
layout: post
title: C3P0连接池没有释放connection
date: 2018-06-11 00:00:00
tags: c3p0
---

在spring中配置了一个C3P0连接池，但是使用的时候发现它卡在一条sql执行上，但把这条sql放进mysql workbench中执行是没有问题。网上说释放一下连接，这里我之前有个误区，就是以为每次查询完一条sql，连接池会自动回收连接。写了个测试程序测试如下：

引入maven依赖

```
<dependency>
    <groupId>com.mchange</groupId>
    <artifactId>c3p0</artifactId>
    <version>0.9.5.2</version>
</dependency>
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.39</version>
</dependency>
```

测试程序如下：

```
package test;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import java.beans.PropertyVetoException;
import java.sql.Connection;
import java.sql.SQLException;

/**
 * Created by VV on 2018/6/11
 */
public class TestC3P0 {

    public static void main(String args[]){
        ComboPooledDataSource dataSource = new ComboPooledDataSource();
        try {
            dataSource.setDriverClass("com.mysql.jdbc.Driver");
            dataSource.setUser("****");
            dataSource.setPassword("****");
            dataSource.setJdbcUrl("jdbc:mysql://****:****/****");
            dataSource.setMaxPoolSize(1);
            dataSource.setInitialPoolSize(1);
            Connection connection = dataSource.getConnection();
            System.out.println(connection);
            connection.close();//如果这里不释放，那下面获取连接就会卡住，程序一直在等待获取
            connection = dataSource.getConnection();
            System.out.println(connection);
        } catch (PropertyVetoException e) {
            e.printStackTrace();
        } catch (SQLException e) {
            e.printStackTrace();
        }

    }
}
```

输出结果：

```
com.mchange.v2.c3p0.impl.NewProxyConnection@18ef96 [wrapping: com.mysql.jdbc.JDBC4Connection@6956de9]
com.mchange.v2.c3p0.impl.NewProxyConnection@ba4d54 [wrapping: com.mysql.jdbc.JDBC4Connection@6956de9]
```

总结：

```
连接connection的关闭close并不是真正的关闭，而是放回连接池中继续使用。
```